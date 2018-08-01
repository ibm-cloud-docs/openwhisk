---

copyright:
  years: 2016, 2018
lastupdated: "2018-06-22"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Création et appel d'actions
{: #openwhisk_actions}

Les actions sont des fragments de code sans état qui s'exécutent sur la plateforme {{site.data.keyword.openwhisk}}. Par exemple, une action peut être utilisée pour détecter les visages dans une image, répondre à une modification de base de données, agréger un ensemble d'appels API ou publier un tweet. Une action peut être écrite en tant que fonction JavaScript, Swift, Python ou PHP, méthode Java ou n'importe quel exécutable compatible binaire, y compris des programmes Go et des programmes exécutables personnalisés dans des conteneurs Docker.
{:shortdesc}

Les actions peuvent être appelées explicitement ou s'exécuter en réponse à un événement. Dans tous les cas, chaque exécution d'une action génère un enregistrement d'activation identifié par un ID d'activation unique. L'entrée pour une action et le résultat d'une action constituent un dictionnaire de paires clé-valeur, où la clé est une chaîne et la valeur est une valeur JSON valide. Les actions peuvent également être composées d'appels à d'autres actions ou à une séquence définie d'actions.

Apprenez à créer, appeler et déboguer des actions dans votre environnement de développement préféré :
* [JavaScript](#creating-and-invoking-javascript-actions)
* [Swift](#creating-swift-actions)
* [Python](#creating-python-actions)
* [Java](#creating-java-actions)
* [PHP](#creating-php-actions)
* [Docker](#creating-docker-actions)
* [Go](#creating-go-actions)
* [Programmes exécutables arbitraires](#creating-actions-arbitrary)

Découvrez également les opérations suivantes :
* [Surveillance de la sortie des actions](#monitor-action-output)
* [Extraction d'actions](#getting-actions)
* [Affichage de la liste des actions](#listing-actions)
* [Suppression d'actions](#deleting-actions)
* [Prise en charge des applications volumineuses](#large-app-support)
* [Accès aux métadonnées d'action dans le corps de l'action](#accessing-action-metadata-within-the-action-body)

## Création et appel d'actions JavaScript
{: #creating-and-invoking-javascript-actions}

Les sections ci-après expliquent comment utiliser des actions dans JavaScript. Vous commencez par créer et appeler une action simple. Ensuite, vous ajoutez des paramètres à une action et vous appelez cette action avec des paramètres. Viennent ensuite les phases de définition et d'appel des paramètres par défaut. Ensuite, vous créez des actions asynchrones et, enfin, vous gérez des séquences d'actions.

### Création et appel d'une action JavaScript simple
{: #openwhisk_single_action_js}

Suivez les étapes et les exemples ci-dessous pour créer votre première action JavaScript.

1. Créez un fichier JavaScript avec le contenu ci-après. Dans cet exemple, nommez le fichier **hello.js**.
  ```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```
  {: codeblock}

  Le fichier JavaScript peut contenir des fonctions supplémentaires. Toutefois, par convention, une fonction appelée **main** doit exister pour fournir le point d'entrée de l'action.

2. Créez une action à partir de la fonction JavaScript ci-dessous. Dans cet exemple, l'action s'appelle **hello**.
  ```
  ibmcloud fn action create hello hello.js
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: created action hello
  ```
  {: screen}

  L'interface de ligne de commande déduit automatiquement le type d'action en utilisant l'extension du fichier source. Pour les fichiers source `.js`, l'action est lancée dans un environnement d'exécution Node.js 6. Vous pouvez également créer une action opérant avec Node.js 8 en mentionnant spécifiquement le paramètre `--kind nodejs:8`. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_javascript_environments) au sujet des environnements Node.js 6 et 8.

3. Répertoriez les actions que vous avez créées :
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

  Vous pouvez voir l'action **hello** que vous avez créée.

4. Après avoir créé l'action, vous pouvez l'exécuter dans le cloud en utilisant la commande **invoke**. Vous pouvez appeler des actions avec un appel bloquant (*blocking*) (de style demande/réponse) ou non bloquant (*non-blocking*) en spécifiant un indicateur dans la commande. Une demande d'appel bloquant _attend_ que le résultat de l'activation soit disponible. Le délai d'attente est inférieur à 60 secondes ou à la [limite de temps](./openwhisk_reference.html#openwhisk_syslimits) de l'action. Le résultat de l'activation est renvoyé s'il est disponible avant la fin du délai d'attente. Sinon, le traitement de l'activation continue dans le système et un ID d'activation est renvoyé de sorte que les résultats puissent être consultés ultérieurement, comme dans le cas des demandes non bloquantes (voir [ici](#monitor-action-output) pour obtenir des astuces sur la surveillance des activations).

  Cet exemple utilise le paramètre `--blocking` :
  ```
  ibmcloud fn action invoke --blocking hello
  ```
  {: pre}

  La commande génère deux informations importantes :
  * L'ID d'activation (`44794bd6aab74415b4e42a308d880e5b`)
  * Le résultat de l'appel s'il est disponible avant la fin du délai d'attente

  **Dans la sortie figure l'ID d'activation :**
  ```
  ok: invoked hello with id 44794bd6aab74415b4e42a308d880e5b
  ```
  {: screen}

  **Résultat de l'appel :**
  ```
  {
      "result": {
          "payload": "Hello world"
      },
      "status": "success",
      "success": true
  }
  ```
  {: screen}

  Dans ce cas, le résultat est la chaîne `Hello world` renvoyée par la fonction JavaScript. L'ID d'activation peut être utilisé pour extraire les journaux ou le résultat de l'appel ultérieurement.

5. Si vous n'avez pas besoin du résultat de l'action immédiatement, vous pouvez omettre l'indicateur `--blocking` pour effectuer un appel non bloquant. Vous pouvez afficher le résultat ultérieurement avec l'ID d'activation.

  Consultez les exemples suivants :
  ```
  ibmcloud fn action invoke hello
  ```
  {: pre}

  **Sortie de la commande :**
  ```
  ok: invoked hello with id 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: screen}

  Maintenant que vous connaissez l'ID d'activation, vous pouvez l'indiquer pour obtenir le résultat de l'action :
  ```
  ibmcloud fn activation result 6bf1f670ee614a7eb5af3c9fde813043
  ```
  {: pre}

  **Résultat de l'action :**
  ```
  {
      "payload": "Hello world"
  }
  ```
  {: screen}

6. Si vous oubliez de noter l'ID d'activation, vous pouvez afficher la liste des activations demandées, de la plus récente à la plus ancienne. Exécutez la commande suivante pour obtenir la liste de vos activations :

  **Répertoriez les activations :**
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

### Création d'actions asynchrones
{: #openwhisk_asynchrony_js}

Les fonctions JavaScript qui s'exécutent de manière asynchrone peuvent renvoyer le résultat d'activation après le retour de la fonction `main` en renvoyant une promesse (objet Promise) dans votre action.

1. Sauvegardez le contenu ci-dessous dans un fichier appelé **asyncAction.js**.
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

  La fonction de rappel de la promesse (objet Promise) prend deux arguments, resolve et reject, qui sont tous les deux des fonctions. L'appel vers `resolve()` satisfait la promesse (objet Promise) et indique que l'activation aboutit.

  Un appel vers `reject()` peut être utilisé pour rejeter la promesse (objet Promise) et signaler que l'activation ne s'est pas terminée normalement.

2. Exécutez les commandes suivantes pour créer l'action et l'appeler.

  Créez une action nommée **asyncAction** :
  ```
  ibmcloud fn action create asyncAction asyncAction.js
  ```
  {: pre}

  Appelez l'action :
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

  Notez que vous avez effectué un appel bloquant d'une action asynchrone.

3. Procédez à l'extraction du journal d'activation pour savoir combien de temps a duré l'activation.

  Pour cela, commencez par afficher l'action pour obtenir l'ID d'activation :
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

  Maintenant récupérez les informations du journal en utilisant l'ID d'activation :
  ```
  ibmcloud fn activation get b066ca51e68c4d3382df2d8033265db0
  ```
  {: pre}

  ```
  {
      "start": 1455881628103,
      "end":   1455881648126,
      ...
  }
  ```
  {: screen}

  Prenez connaissance des horodatages de début (`start`) et de fin (`end`) dans l'enregistrement d'activation : vous constatez que cette activation a pris un peu plus de 2 secondes pour s'exécuter.

### Utilisation d'actions pour appeler une API externe
{: #openwhisk_apicall_action}

Jusqu'à présent, les exemples fournis sont des fonctions JavaScript autonomes. Vous pouvez également créer une action qui appelle une API externe.

L'exemple suivant appelle le service APOD (Astronomy Picture of the Day) de la NASA qui fournit tous les jours une image unique de notre univers.

1. Sauvegardez le contenu suivant dans un fichier nommé **apod.js**.
  ```javascript
  var url = "https://api.nasa.gov/planetary/apod?api_key=NNKOjkoul8n1CH18TWA9gwngW1s1SmjESPjNoUFo";

  $.ajax({
    url: url,
    success: function(result){
    if("copyright" in result) {
      $("#copyright").text("Image Credits: " + result.copyright);
    }
    else {
      $("#copyright").text("Image Credits: " + "Public Domain");
    }

    if(result.media_type == "video") {
      $("#apod_img_id").css("display", "none");
      $("#apod_vid_id").attr("src", result.url);
    }
    else {
      $("#apod_vid_id").css("display", "none");
      $("#apod_img_id").attr("src", result.url);
    }
    $("#reqObject").text(url);
    $("#returnObject").text(JSON.stringify(result, null, 4));
    $("#apod_explaination").text(result.explanation);
    $("#apod_title").text(result.title);
  }
  });
  ```
  {: codeblock}

  Un appel de l'API de la NASA APOD est émis et extrait les zones du résultat au format JSON. La rubrique [Références](./openwhisk_reference.html#openwhisk_ref_javascript_environments) fournit les détails des packages Node.js que vous pouvez utiliser dans vos actions.

2. Exécutez les commandes suivantes pour créer l'action et l'appeler.

  Créez l'action nommée **apod** :
  ```
  ibmcloud fn action create apod apod.js
  ```
  {: pre}

  Appelez l'action **apod** :
  ```
  ibmcloud fn action invoke --result apod
  ```
  {: pre}

  **Objet renvoyé :**
  ```
  {
    "copyright": "Eric Houck",
    "date": "2018-03-28",
    "explanation": "Does an alignment like this occur only once in a blue moon? No, although it was during a blue moon that this single-shot image was taken.  During a full moon that happened to be the second of the month -- the situation that defines a blue moon -- the photographer created the juxtaposition in late January by quickly moving around to find just the right spot to get the background Moon superposed behind the arc of a foreground tree.  Unfortunately, in this case, there seemed no other way than getting bogged down in mud and resting the camera on a barbed-wire fence.  The arc in the oak tree was previously created by hungry cows in Knight's Ferry, California, USA.  Quirky Moon-tree juxtapositions like this can be created during any full moon though, given enough planning and time.  Another opportunity will arise this weekend, coincidently during another blue moon. Then, the second blue moon in 2018 will occur, meaning that for the second month this year, two full moons will appear during a single month (moon-th).  Double blue-moon years are relatively rare, with the last occurring in 1999, and the next in 2037.",
    "hdurl": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_1799.jpg",
    "media_type": "image",
    "service_version": "v1",
    "title": "Blue Moon Tree",
    "url": "https://apod.nasa.gov/apod/image/1803/MoonTree_Houck_960.jpg"
  }
  ```
  {: screen}

### Conditionnement d'une action dans un module Node.js
{: #openwhisk_js_packaged_action}

Comme alternative à l'écriture de l'ensemble du code de votre action dans un seul fichier source JavaScript, vous pouvez écrire une action en tant que package `npm`. Prenez par exemple un répertoire contenant les fichiers suivants :

**package.json :**
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

**index.js :**
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

Pour créer une action {{site.data.keyword.openwhisk_short}} à partir de ce package :

1. Installez toutes les dépendances en local :
  ```
  npm install
  ```
  {: pre}

2. Créez une archive `.zip` contenant tous les fichiers (notamment toutes les dépendances) :
  ```
  zip -r action.zip *
  ```
  {: pre}

  L'utilisation d'une action Windows Explorer pour créer le fichier zip génère une structure incorrecte. Les actions zip {{site.data.keyword.openwhisk_short}} nécessitent que `package.json` soit placé à la racine du fichier zip, alors que l'Explorateur Windows le place dans un dossier imbriqué. L'option la plus sûre consiste à utiliser la commande `zip` de ligne de commande.
  {: tip}

3. Créez l'action :
  ```
  ibmcloud fn action create packageAction --kind nodejs:6 action.zip
  ```
  {: pre}

  Lorsque vous créez une action depuis une archive `.zip` via l'outil d'interface de ligne de commande, vous devez fournir explicitement une valeur pour l'indicateur `--kind` à l'aide de `nodejs:6` ou `nodejs:8`.

4. Vous pouvez appeler l'action à l'instar de n'importe quelle autre action :
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

Enfin, notez qu'alors que la plupart des packages `npm` installent des sources JavaScript avec la commande `npm install`, certains installent et compilent également des artefacts binaires. Actuellement, le téléchargement de fichier archive ne prend pas en charge les dépendances binaires et n'admet que les dépendances JavaScript. Les appels d'action risquent d'échouer si l'archive comprend des dépendances binaires.

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
    "deploy": "ibmcloud fn action update my-action dist/bundle.js --kind nodejs:8"
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

Si le nom de votre fonction est `main`, utilisez plutôt cette syntaxe :
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

  Le fichier `dist/bundle.js` est créé et utilisé pour un déploiement en tant que code source de l'action.

3. Créez l'action à l'aide du script `npm` ou de l'interface de ligne de commande.

  Utilisation du script `npm` :
  ```
  npm run deploy
  ```
  {: pre}

  Utilisation de l'interface de ligne de commande :
  ```
  ibmcloud fn action update my-action dist/bundle.js
  ```
  {: pre}

Enfin, le fichier bundle qui est généré par `webpack` ne prend pas en charge des dépendances binaires mais plutôt des dépendances JavaScript. Par conséquent, les appels d'action échouent si le bundle dépend de dépendances binaires, car cela n'est pas inclus avec le fichier `bundle.js`.

## Création de séquences d'actions
{: #openwhisk_create_action_sequence}

Vous pouvez créer une action qui assemble des actions dans une séquence.

Plusieurs actions d'utilitaire sont fournies dans un package appelé `/whisk.system/utils` que vous pouvez utiliser pour créer votre première séquence. Pour en savoir sur les packages, voir la section [Packages](./openwhisk_packages.html).

1. Affichez les actions du package `/whisk.system/utils`.
  ```
  ibmcloud fn package get --summary /whisk.system/utils
  ```
  {: pre}

  Exemple de sortie :
  ```
  package /whisk.system/utils: Building blocks that format and assemble data
   action /whisk.system/utils/head: Extract prefix of an array
   action /whisk.system/utils/split: Split a string into an array
   action /whisk.system/utils/sort: Sorts an array
   action /whisk.system/utils/echo: Returns the input
   action /whisk.system/utils/date: Current date and time
   action /whisk.system/utils/cat: Concatenates input into a string
  ```
  {: screen}

  Vous utiliserez dans cet exemple les actions `split` et `sort`.

2. Créez une séquence d'actions pour que le résultat d'une action soit transmis sous forme d'argument à l'action suivante.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

  Cette séquence d'actions convertit des lignes de texte en tableau et trie les lignes.

3. Appelez l'action :
  ```
  ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "length": 3,
      "lines": [
          "Is full of regret.",
          "Over-ripe sushi,",
          "The Master"
      ]
  }
  ```
  {: screen}

  Vous constatez que les lignes sont triées dans le résultat.

**Remarque** : les paramètres transmis entre les actions de la séquence sont explicites, sauf les paramètres par défaut.
Par conséquent, les paramètres qui sont transmis à la séquence d'actions ne sont disponibles que pour la première action de la séquence. Le résultat de la première action dans la séquence devient l'objet JSON d'entrée de la deuxième action de la séquence (et ainsi de suite). Cet objet ne comporte pas les paramètres qui sont transmis initialement à la séquence sauf si la première action les inclut explicitement dans son résultat. Les paramètres d'entrée d'une action sont fusionnés avec ses paramètres par défaut, les premiers étant prioritaires et remplaçant tout paramètre par défaut correspondant. Pour plus d'informations sur l'appel des séquences d'actions avec plusieurs paramètres nommés, voir [Définir les paramètres par défaut sur une action](./parameters.html#default-params-action).

## Création d'actions Python
{: #creating-python-actions}

Le processus de création d'actions Python est similaire au processus de création d'actions JavaScript. Les sections ci-après vous expliquent comment créer et appeler une action Python unique, et comment ajouter des paramètres à cette action.

### Création et appel d'une action Python
{: #openwhisk_actions_python_invoke}

Une action est simplement une fonction Python de premier niveau. Par exemple, créez un fichier nommé **hello.py** contenant le code source suivant :
```python
def main(args):
    name = args.get("name", "stranger")
    greeting = "Hello " + name + "!"
    print(greeting)
        return {"greeting": greeting}
```
{: codeblock}

Les actions Python consomment et produisent toujours un dictionnaire. Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée explicitement pour créer l'action avec l'interface CLI `wsk` en utilisant `--main`, comme pour n'importe quel type d'action.

Vous pouvez créer une action {{site.data.keyword.openwhisk_short}} appelée **helloPython** depuis cette fonction :
```
ibmcloud fn action create helloPython hello.py
```
{: pre}

L'interface CLI déduit automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.py`, l'action est lancée dans un environnement d'exécution Python 2. Vous pouvez également créer une action opérant avec Python 3 en mentionnant explicitement le paramètre `--kind python:3`. Il existe également un environnement d'exécution Python 3 avec le type `python-jessie:3` qui contient des packages supplémentaires pour les services IBM Cloud, telles qu'IBM Cloudant, IBM DB2, IBM COS et IBM Watson.
Pour plus d'informations sur les packages inclus dans cet environnement d'exécution Python 3, voir la section de [référence](./openwhisk_reference.html#openwhisk_ref_python_environments) de l'environnement d'exécution Python.

L'appel d'action est identique pour les actions Python et pour les actions JavaScript :
```
ibmcloud fn action invoke --result helloPython --param name World
```
{: pre}

Exemple de sortie :
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

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
ibmcloud fn action create helloPython --kind python:3 helloPython.zip
```
{: pre}

Ces étapes s'appliquent à Python 3 (avec le type `python:3`), mais vous pouvez faire de même avec les autres types Python `python:2` ou `python-jessie:3`.

### Conditionnement d'actions Python avec un environnement virtuel dans des fichiers zip
{: #openwhisk_actions_python_virtualenv}

Un autre moyen de conditionner des dépendances Python consiste à utiliser un environnement virtuel (`virtualenv`), ce qui vous permet de lier des packages supplémentaires pouvant être installés en utilisant [`pip`](https://packaging.python.org/installing/), par exemple. 

Comme avec la prise en charge de fichier zip de base, le nom du fichier source qui contient le point d'entrée principal doit être `__main__.py`. Plus précisément, le contenu de `__main__.py` est la fonction main, par conséquent, pour cet exemple, vous pouvez renommer `hello.py` `__main__.py` à partir de la section précédente. De plus, le répertoire virtualenv doit s'appeler `virtualenv`. Vous trouverez ci-dessous un exemple de scénario d'installation des dépendances, de conditionnement de ces dernières dans un environnement virtuel et de création d'une action OpenWhisk compatible.

Pour assurer la compatibilité avec le conteneur d'exécution OpenWhisk, les installations de package dans un environnement virtuel (virtualenv) doivent être effectuées dans l'environnement cible en utilisant l'image correspondante pour le type.
- Pour le type `python:2`, utilisez l'image Docker `openwhisk/python2action`.
- Pour le type `python:3`, utilisez l'image Docker `openwhisk/python3action`.
- Pour le type `python-jessie:3`, utilisez l'image Docker `ibmfunctions/action-python-v3`.

1. Avec un fichier [requirements.txt ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) contenant les versions et les modules `pip` à installer, exécutez les commandes suivantes pour installer les dépendances et créer un environnement virtuel à l'aide d'une image Docker compatible :
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

2. Archivez le répertoire virtualenv ainsi que les éventuels fichiers Python supplémentaires :
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

3. Créez l'action **helloPython** :
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Ajoutez uniquement au fichier `requirements.txt` les modules qui ne font pas partie de l'environnement d'exécution sélectionné. Cela permet de conserver une taille minimale pour l'environnement virtuel.
{: tip}

## Création d'actions PHP
{: #creating-php-actions}

Le processus de création d'actions PHP est similaire à celui de création d'actions JavaScript. Les sections ci-après expliquent comment créer et appeler une action Java PHP unique, et comment ajouter des paramètres à cette action.

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
{: codeblock}

Les actions PHP consomment et renvoient toujours un tableau associatif. Par défaut, la méthode d'entrée pour l'action est `main` par défaut, mais elle peut être spécifiée explicitement lorsque vous créez l'action avec l'interface de ligne de commande `ibmcloud fn` en utilisant `--main`, comme pour n'importe quel autre type d'action.

Vous pouvez créer une action {{site.data.keyword.openwhisk_short}} appelée **helloPHP** depuis cette fonction comme suit :
```
ibmcloud fn action create helloPHP hello.php
```
{: pre}

L'interface CLI déduit automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.php`, l'action est lancée dans un environnement d'exécution PHP 7.1. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_php) au sujet de PHP.

L'appel d'action est identique pour les actions PHP et les actions JavaScript :
```
ibmcloud fn action invoke --result helloPHP --param name World
```
{: pre}

Exemple de sortie :
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

### Conditionnement d'actions PHP dans des fichiers zip
{: #openwhisk_actions_php_zip}

Vous pouvez créer un package pour une action PHP, ainsi que pour d'autres fichiers et packages dépendants, dans un fichier zip.
Le nom du fichier source contenant le point d'entrée (par exemple, `main`) doit être `index.php`.

Par exemple, pour créer une action qui inclut un second fichier nommé `helper.php`, créez d'abord une archive contenant vos fichiers source :
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}

Créez ensuite l'action **helloPHP** :
```bash
ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
```
{: pre}

## Création d'actions Swift
{: #creating-swift-actions}

Le processus de création d'actions Swift est similaire au processus de création d'actions JavaScript. Les sections suivantes expliquent comment créer et appeler une action Swift unique et conditionner cette action dans un fichier zip.

Vous pouvez également utiliser le site [Online Swift Playground](http://online.swiftplayground.run) pour tester votre code Swift sans avoir à installer Xcode sur votre machine.

**Attention :** les actions Swift s'exécutent dans un environnement Linux. Swift sous Linux est en cours
de développement et OpenWhisk utilise en principe la dernière édition disponible qui n'est pas forcément stable. En outre, il se peut que la version de Swift utilisée avec OpenWhisk ne corresponde pas aux versions de Swift provenant d'éditions stables de Xcode sous MacOS.

### Création et appel d'une action

#### Swift 3
Une action est simplement une fonction Swift de niveau supérieur. Par exemple, créez un fichier appelé
**hello.swift** avec le contenu suivant :

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

Dans cet exemple, l'action Swift consomme et produit un dictionnaire.

Vous pouvez créer une action OpenWhisk appelée **helloSwift** depuis cette fonction,
comme suit :
```
ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
```
{: pre}

#### Swift 4

Nouveauté dans Swift 4 : en plus de la signature de la fonction main ci-dessus, il existe deux autres signatures prêtes à l'emploi qui bénéficient du type [Codable](https://developer.apple.com/documentation/swift/codable). Pour en savoir plus sur les types de données codables et décodables permettant la compatibilité avec des représentations externes, telles que JSON, cliquez [ici](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

Dans l'exemple suivant figure un paramètre d'entrée en tant qu'entrée codable (**Codable Input**) avec le champ `name`, et un paramètre de sortie codable (**Codable Output**) est renvoyé avec un champ `greetings`.
```swift
struct Input: Codable {
    let name: String?
}
struct Output: Codable {
    let greeting: String
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello \(param.name ?? "stranger")!")
    print("Log greeting:\(result.greeting)")
    completion(result, nil)
}
```
{: codeblock}

Dans cet exemple, l'action Swift consomme et produit un type Codable.
Si vous n'avez pas besoin de traiter d'entrée, vous pouvez utiliser cette signature de fonction qui ne prend pas d'entrée mais uniquement une sortie Codable.
```swift
struct Output: Codable {
    let greeting: String
}
func main(completion: (Output?, Error?) -> Void) -> Void {
    let result = Output(greeting: "Hello OpenWhisk!")
    completion(result, nil)
}
```
{: codeblock}

Vous pouvez créer une action OpenWhisk appelée `helloSwift` depuis cette fonction,
comme suit :
```
ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
```
{: pre}

Voir le [document de référence](./openwhisk_reference.html#swift-actions) Swift pour plus d'informations sur l'environnement d'exécution Swift.

L'appel d'action est identique pour les actions Swift et les actions JavaScript :
```
ibmcloud fn action invoke --result helloSwift --param name World
```
{: pre}

Exemple de sortie :
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

Pour en savoir plus sur les paramètres, voir la rubrique [Utilisation des paramètres](./parameters.html).

### Conditionnement d'une action sous forme d'exécutable Swift
{: #packaging-an-action-as-a-swift-executable}

Lorsque vous créez une action Swift OpenWhisk avec un fichier source Swift, celui-ci doit être compilé en fichier binaire avant l'exécution de l'action. Après quoi, les appels ultérieurs de l'action sont beaucoup plus rapides jusqu'à ce que le conteneur hébergeant votre action soit purgé. Ce délai est dénommé délai de démarrage à froid.

Pour éviter ce délai, vous pouvez compiler votre fichier Swift en binaire, puis le télécharger dans OpenWhisk sous forme de fichier zip. Comme vous avez besoin de l'échafaudage OpenWhisk, la manière la plus facile de créer le binaire consiste à le générer dans le même environnement que celui où il sera exécuté.

### Utilisation d'un script pour générer une action conditionnée avec Swift

Vous pouvez utiliser un script pour automatiser le conditionnement d'une action. Créez un fichier script nommé `compile.sh` à l'aide de l'exemple de code suivant.
```bash
#!/bin/bash
set -ex

if [ -z "$1" ] ; then
    echo 'Error: Missing action name'
    exit 1
fi
if [ -z "$2" ] ; then
    echo 'Error: Missing kind, for example swift:4.1'
    exit 2
fi
OUTPUT_DIR="build"
if [ ${2} == "swift:3.1.1" ]; then
  BASE_PATH="/swift3Action"
  DEST_SOURCE="$BASE_PATH/spm-build"
  RUNTIME="openwhisk/action-swift-v3.1.1"
elif [ ${2} == "swift:4.1" ]; then
  RUNTIME="ibmfunctions/action-swift-v4.1"
  BASE_PATH="/swift4Action"
  DEST_SOURCE="/$BASE_PATH/spm-build/Sources/Action"
else
  echo "Error: Kind $2 not recognize"
  exit 3
fi
DEST_PACKAGE_SWIFT="$BASE_PATH/spm-build/Package.swift"

BUILD_FLAGS=""
if [ -n "$3" ] ; then
    BUILD_FLAGS=${3}
fi

echo "Using runtime $RUNTIME to compile swift"
docker run --rm --name=compile-ow-swift -it -v "$(pwd):/owexec" $RUNTIME bash -ex -c "

if [ -f \"/owexec/$OUTPUT_DIR/$1.zip\" ] ; then
    rm \"/owexec/$OUTPUT_DIR/$1.zip\"
fi

echo 'Setting up build...'
cp /owexec/actions/$1/Sources/*.swift $DEST_SOURCE/

# action file can be either {action name}.swift or main.swift
if [ -f \"$DEST_SOURCE/$1.swift\" ] ; then
    echo 'renaming $DEST_SOURCE/$1.swift $DEST_SOURCE/main.swift'
    mv \"$DEST_SOURCE/$1.swift\" $DEST_SOURCE/main.swift
fi
# Add in the OW specific bits
cat $BASE_PATH/epilogue.swift >> $DEST_SOURCE/main.swift
echo '_run_main(mainFunction:main)' >> $DEST_SOURCE/main.swift

# Only for Swift4
if [ ${2} != "swift:3.1.1" ]; then
  echo 'Adding wait to deal with escaping'
  echo '_ = _whisk_semaphore.wait(timeout: .distantFuture)' >> $DEST_SOURCE/main.swift
fi

echo \"Compiling $1...\"
cd /$BASE_PATH/spm-build
cp /owexec/actions/$1/Package.swift $DEST_PACKAGE_SWIFT
# we have our own Package.swift, do a full compile
swift build ${BUILD_FLAGS} -c release

echo 'Creating archive $1.zip...'
#.build/release/Action
mkdir -p /owexec/$OUTPUT_DIR
zip \"/owexec/$OUTPUT_DIR/$1.zip\" .build/release/Action
"
```
{: codeblock}

Le script suppose que vous ayez un répertoire appelé `actions`, avec chaque répertoire de niveau supérieur représentant une action.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

- Créez le fichier `Package.swift` pour ajouter des dépendances. **Remarque :** la syntaxe est différente comparée à celle des outils Swift 3 à Swift 4.

  Exemple de syntaxe de Swift 3 :
  ```swift
  import PackageDescription

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
  {: codeblock}

  Exemple de syntaxe de Swift 4 :
  ```swift
  // swift-tools-version:4.0
  import PackageDescription

  let package = Package(
      name: "Action",
      products: [
        .executable(
          name: "Action",
          targets:  ["Action"]
        )
      ],
      dependencies: [
        .package(url: "https://github.com/apple/example-package-deckofplayingcards.git", .upToNextMajor(from: "3.0.0"))
      ],
      targets: [
        .target(
          name: "Action",
          dependencies: ["DeckOfPlayingCards"],
          path: "."
        )
      ]
  )
  ```
  {: codeblock}

  Comme vous pouvez le constater, cet exemple ajoute `example-package-deckofplayingcards` sous forme de dépendance. Observez que `CCurl`, `Kitura-net` et `SwiftyJSON` sont fournis dans l'action Swift standard. Par conséquent, vous devez les inclure dans votre propre fichier `Package.swift` uniquement pour les actions Swift 3.

- Générez l'action en exécutant la commande suivante pour une action Swift 3 :
  ```
  bash compile.sh hello swift:3.1.1
  ```
  {: pre}

  Pour compiler avec Swift 4, utilisez `swift:4.1` à la place de `swift:3.1.1` :
  ```
  bash compile.sh hello swift:4.1
  ```
  {: pre}

  Cette commande a créé le fichier `hello.zip` dans le `build`.

- Transférez-le vers OpenWhisk avec le nom d'action **helloSwifty**. Pour Swift 3, utilisez le paramètre kind `swift:3.1.1`
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
  ```
  {: pre}

  Pour Swift 4, utilisez le paramètre kind `swift:3.1.1` :
  ```
  ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
  ```
  {: pre}

- Pour apprécier le gain en rapidité ainsi obtenu, exécutez la commande suivante :
  ```
  ibmcloud fn action invoke helloSwiftly --blocking
  ```
  {: pre}

  La durée d'exécution de l'action figure dans la propriété "duration" et vous pouvez la comparer à la durée d'exécution avec une étape de compilation dans l'action **hello**.

### Erreur lors du traitement dans Swift 4

Avec le nouveau gestionnaire completionHandler Codable, vous pouvez transmettre le paramètre Error pour indiquer qu'un incident s'est produit dans votre action.
Le [traitement des erreurs dans Swift](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) est comparable au traitement des exceptions dans d'autres langages, avec l'utilisation des mots clés `try, catch` et `throw`.

Le fragment suivant montre un exemple de traitement d'erreur :
```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
func main(param: Input, completion: (Output?, Error?) -> Void) -> Void {
    // Return real error
    do{
        throw VendingMachineError.insufficientFunds(coinsNeeded: 5)
    } catch {
        completion(nil, error)
    }
}
```
{: codeblock}

## Création d'actions Java
{: #creating-java-actions}

Le processus de création d'actions Java est similaire au processus de création d'actions JavaScript et Swift. Les sections ci-après expliquent comment créer et appeler une action Java unique, et comment ajouter des paramètres à cette action.

Pour pouvoir compiler, tester et archiver des fichiers Java, vous devez avoir installé un logiciel [JDK 8](http://openjdk.java.net/install) en local.

### Création et appel d'une action Java
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

Vous pouvez créer une action {{site.data.keyword.openwhisk_short}} appelée **helloJava** à partir de ce fichier JAR,
comme suit :
```
ibmcloud fn action create helloJava hello.jar --main Hello
```
{: pre}

Lorsque vous utilisez la ligne de commande et un fichier source `.jar`, il n'est pas nécessaire de spécifier que vous créez une action Java ; l'outil le détermine à partir de l'extension de fichier.

Vous devez spécifier le nom de la classe principale à l'aide de `--main`. Une classe principale est éligible si elle implémente une méthode `main` statique. Si la classe ne se trouve pas dans le package par défaut, utilisez le nom de classe qualifié complet Java, par exemple, `--main com.example.MyMain`.

Si besoin, vous pouvez également personnaliser le nom de méthode de votre action Java. Pour cela, spécifiez le nom de méthode qualifié complet Java de votre action, par exemple, `--main com.example.MyMain#methodName`.

L'appel d'action est identique pour les actions Java et les actions Swift et JavaScript :
```
ibmcloud fn action invoke --result helloJava --param name World
```
{: pre}

Exemple de sortie :
```
  {
      "greeting": "Hello World!"
  }
```
{: screen}

## Création d'actions Docker
{: #creating-docker-actions}

Avec les actions Docker {{site.data.keyword.openwhisk_short}}, vous pouvez écrire vos actions dans n'importe quel langage.

Votre code est compilé dans un fichier binaire exécutable et imbriqué dans une image Docker. Le programme binaire interagit avec le système en prenant l'entrée dans `stdin` et en répondant par le biais de `stdout`.

Au préalable, vous devez disposer d'un compte Docker Hub.  Pour configurer un ID et un compte Docker gratuits, visitez le site [Docker Hub](https://hub.docker.com).

Dans les instructions qui suivent, l'ID utilisateur Docker est `janesmith` et le mot de passe est `janes_password`.  Si l'on part du principe que l'interface de ligne de commande est configurée, il reste trois étapes à effectuer pour configurer un fichier binaire personnalisé pouvant être utilisé par {{site.data.keyword.openwhisk_short}}. Ensuite, l'image Docker téléchargée peut être utilisée en tant qu'action.

1. Téléchargez le squelette Docker. Vous pouvez le télécharger et l'installer via l'interface de ligne de commande en procédant comme suit :
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

  A présent, le squelette Docker est installé dans le répertoire en cours.
  ```
  ls dockerSkeleton/
  ```
  {: pre}

  Exemple de sortie :
  ```
  Dockerfile      README.md       buildAndPush.sh example.c
  ```
  {: screen}

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
  Afin d'ajouter un code et des dépendances supplémentaires, vous pouvez le cas échéant personnaliser le document `Dockerfile` pour générer votre fichier exécutable.
  Le fichier binaire doit se trouver dans le conteneur dans le répertoire `/action/exec`.

  Le fichier exécutable reçoit un argument simple depuis la ligne de commande. Il s'agit d'une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action. Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  Par convention, la dernière ligne de la sortie _doit_ être un objet JSON transformé en chaîne qui représente le résultat de l'action.

3. Générez l'image Docker et téléchargez-la à l'aide d'un script fourni. Vous devez d'abord exécuter `docker login` pour l'authentification, puis exécuter le script avec le nom d'image de votre choix.
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

  Notez qu'une partie du fichier `example.c` est compilée dans le cadre du processus de génération de l'image Docker ; par conséquent, il n'est pas nécessaire que C soit compilé sur votre machine. En fait, sauf si vous le compilez sur une machine hôte compatible, le fichier binaire ne pourra pas s'exécuter dans le conteneur car les formats ne concorderont pas.

  Votre conteneur Docker peut désormais être utilisé en tant qu'action {{site.data.keyword.openwhisk_short}} :
  ```
  ibmcloud fn action create example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Vous remarquerez l'utilisation de `--docker` pour créer une action. On suppose que toutes les images Docker sont hébergées dans Docker Hub. L'action peut être appelée comme n'importe quelle autre action {{site.data.keyword.openwhisk_short}}.
  ```
  ibmcloud fn action invoke --result example --param payload Rey
  ```
  {: pre}

  **Résultat de l'appel :**
  ```
  {
      "args": {
          "payload": "Rey"
      },
      "msg": "Hello from arbitrary C program!"
  }
  ```
  {: screen}

  Afin de mettre à jour l'action Docker, exécutez `buildAndPush.sh` pour télécharger l'image la plus récente dans le dockerhub. Ainsi, le système pourra extraire votre nouvelle image Docker à la prochaine exécution du code pour votre action. Si aucun conteneur n'est en cours d'exécution, les nouveaux appels utilisent la nouvelle image Docker. Toutefois, s'il existe un conteneur en cours d'exécution utilisant une version précédente de votre image Docker, les nouveaux appels continuent d'utiliser cette image sauf si vous exécutez `ibmcloud fn action update`. Cette commande indique au système que pour les nouveaux appels, une commande docker pull doit être exécutée afin d'obtenir votre nouvelle image Docker.

  **Transférez l'image la plus récente à Docker Hub :**
  ```
  ./buildAndPush.sh janesmith/blackboxdemo
  ```
  {: pre}

  **Mettez à jour l'action de sorte que les nouveaux appels commencent à utiliser la nouvelle image :***
  ```
  ibmcloud fn action update example --docker janesmith/blackboxdemo
  ```
  {: pre}

  Des informations sur la création d'actions Docker sont disponibles dans la section [Références](./openwhisk_reference.html#openwhisk_ref_docker).

  La version antérieure de l'interface de ligne de commande prenait en charge l'option `--docker` sans paramètre et le nom d'image était un argument de position. Pour permettre aux actions Docker d'accepter des données d'initialisation en utilisant un fichier (zip), normalisez l'expérience utilisateur pour les actions Docker de sorte que, si un argument de position est présent, il doit s'agir d'un fichier (par exemple, un fichier zip) à la place. Le nom de l'image doit être spécifié à la suite de l'option `--docker`. Suite aux commentaires en retour des utilisateurs, l'argument `--native` a été inclus comme abréviation de `--docker openwhisk/dockerskeleton` afin que les exécutables qui s'exécutent dans le SDK d'action Docker standard soient plus faciles à créer et à déployer.

  Par exemple, ce tutoriel crée un exécutable binaire dans le conteneur situé sous `/action/exec`. Si vous copiez ce fichier sur votre système de fichiers local et que vous le compressez dans un fichier `exec.zip`, vous pouvez utiliser les commandes suivantes pour créer une action Docker qui reçoit l'exécutable en tant que données d'initialisation.

  **Créez l'action à partir du fichier zip :**
  ```
  ibmcloud fn action create example exec.zip --native
  ```
  {: pre}

  Qui est équivalente à la commande suivante :
  ```
  ibmcloud fn action create example exec.zip --docker openwhisk/dockerskeleton
  ```
  {: pre}

## Création d'actions Go
{: #creating-go-actions}

L'option `--native` permet de conditionner n'importe quel exécutable en tant qu'action. Cela fonctionne pour Go, par exemple. A l'instar des actions Docker, l'exécutable Go reçoit un argument unique de la part de la ligne de commande. Il s'agit d'une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action. Le programme peut effectuer la journalisation dans `stdout` ou `stderr`. Par convention, la dernière ligne de la sortie _doit_ être un objet JSON transformé en chaîne qui représente le résultat de l'action.

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
{: codeblock}

Sauvegardez le code ci-dessus dans un fichier `sample.go` et soumettez-le à une compilation croisée pour OpenWhisk. L'exécutable doit s'appeler `exec`.
```bash
GOOS=linux GOARCH=amd64 go build -o exec
zip exec.zip exec
ibmcloud fn action create helloGo --native exec.zip
```
{: codeblock}

L'action peut être exécutée comme n'importe quelle autre action.
```bash
ibmcloud fn action invoke helloGo -r -p name gopher
{
    "msg": "Hello, gopher!"
}
```

Les journaux sont eux aussi extraits de manière similaire.
```bash
ibmcloud fn activation logs --last --strip
my first Go action.
```

## Création d'actions à l'aide d'exécutables arbitraires
{: #creating-actions-arbitrary}

Comme vous pouvez le voir, avec l'option `--native`, _n'importe quel_ exécutable peut être exécuté en tant qu'action OpenWhisk. Cela inclut les scripts `bash` ou les binaires ayant fait l'objet d'une compilation croisée. Pour ces derniers, il existe une contrainte liée au fait que les binaires doivent être compatibles avec l'image `openwhisk/dockerskeleton`.

## Surveillance de la sortie des actions
{: #monitor-action-output}

Des actions {{site.data.keyword.openwhisk_short}} peuvent être appelées par d'autres utilisateurs en réponse à divers événements, ou dans le cadre d'une séquence d'actions. Dans ces cas, il peut être utile de surveiller les appels.

Vous pouvez utiliser l'interface de ligne de commande {{site.data.keyword.openwhisk_short}} pour surveiller la sortie des actions, au fur et à mesure qu'elles sont appelées.

1. Emettez la commande suivante depuis un interpréteur de commandes :
  ```
  ibmcloud fn activation poll
  ```
  {: pre}

  Cette commande démarre une boucle d'interrogation qui consulte en permanence les journaux des activations.

2. Passez dans une autre fenêtre et appelez une action :
  ```
  ibmcloud fn action invoke /whisk.system/samples/helloWorld --param payload Bob
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: invoked /whisk.system/samples/helloWorld with id 7331f9b9e2044d85afd219b12c0f1491
  ```
  {: screen}

3. Observez le journal d'activation dans la fenêtre d'interrogation :
  ```
  Activation: helloWorld (7331f9b9e2044d85afd219b12c0f1491)
    2016-02-11T16:46:56.842065025Z stdout: hello bob!
  ```
  {: screen}

  De même, chaque fois que vous exécutez l'utilitaire d'interrogation (poll), vous pouvez consulter en temps réel les journaux de n'importe quelle action exécutée pour votre compte dans OpenWhisk.

## Extractions d'actions
{: #getting-actions}

Les métadonnées qui décrivent des actions existantes peuvent être extraites en utilisant la commande `ibmcloud fn action` get.

**Commande :**
```
ibmcloud fn action get hello
```

***Résultat :**
```
ok: got action hello
{
    "namespace": "user@email.com",
    "name": "hello",
    "version": "0.0.1",
    "exec": {
        "kind": "nodejs:6",
        "binary": false
    },
    "annotations": [
        {
            "key": "exec",
            "value": "nodejs:6"
        }
    ],
    "limits": {
        "timeout": 60000,
        "memory": 256,
        "logs": 10
    },
    "publish": false
}
```
{: screen}

### Obtention d'une adresse URL d'action
{: #get-action-url}

Une action peut être appelée en utilisant l'interface REST par le biais d'une demande HTTP. Pour obtenir une adresse URL d'action, exécutez la commande suivante :
```
ibmcloud fn action get actionName --url
```
{: pre}

Une adresse URL au format suivant est renvoyée pour les actions standard :
```
ok: got action actionName
https://${APIHOST}/api/v1/namespaces/${NAMESPACE}/actions/actionName
```
{: screen}

Pour les [actions Web](./openwhisk_webactions.html), une adresse URL est renvoyée au format suivant :
```
ok: got action actionName
https://${APIHOST}/api/v1/web/${NAMESPACE}/${PACKAGE}/actionName
```
{: screen}

**Remarque :** pour les actions standard, l'authentification doit être fournie lorsque ces actions sont appelées par le biais d'une demande HTTPS. Pour plus d'informations concernant l'appel d'actions à l'aide de l'interface REST, voir le [document de référence de l'API REST](https://console.bluemix.net/apidocs/98-cloud-functions?&language=node#introduction).

### Sauvegarde de code d'action
{: #save-action}

Le code associé à une action existante peut être récupéré et sauvegardé en local. Toutes les actions peuvent être sauvegardées à l'exception des séquences et des actions Docker.

1. Sauvegardez le code d'action dans un nom de fichier qui correspond à un nom d'action existant dans le répertoire de travail en cours. Une extension de fichier correspondant au type d'action est utilisée, ou une extension de type .zip sera utilisée pour un code d'action qui correspond à un fichier zip. 
  ```
  ibmcloud fn action get actionName --save
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: saved action code to /absolutePath/currentDirectory/actionName.js
  ```
  {: screen}

2. Au lieu d'autoriser l'interface de ligne de commande à déterminer l'emplacement de destination du code à sauvegarder, un chemin de fichier personnalisé, un nom de fichier et une extension peuvent être fournis en utilisant l'indicateur `--save-as`.
  ```
  ibmcloud fn action get actionName --save-as codeFile.js
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: saved action code to /absolutePath/currentDirectory/codeFile.js
  ```
  {: screen}

## Affichage de la liste des actions
{: #listing-actions}

Vous pouvez afficher la liste de toutes les actions créées à l'aide de la commande suivante :
```
ibmcloud fn action list
```
{: pre}

A mesure que vous créez d'autres actions, cette liste s'allonge et il peut être opportun de regrouper les actions associées dans des [packages](./openwhisk_packages.html). Pour filtrer votre liste d'actions de manière à afficher uniquement celles qui appartiennent à un package spécifique, utilisez la syntaxe de commande suivante :
```
ibmcloud fn action list [PACKAGE NAME]
```
{: pre}

## Suppression d'actions
{: #deleting-actions}

Vous pouvez procéder à un nettoyage en supprimant les actions que vous ne voulez pas utiliser.

1. Exécutez la commande suivante pour supprimer une action :
  ```
  ibmcloud fn action delete hello
  ```
  {: pre}

  Exemple de sortie :
  ```
  ok: deleted hello
  ```
  {: screen}

2. Vérifiez que l'action n'apparaît plus dans la liste des actions.
  ```
  ibmcloud fn action list
  ```
  {: pre}

  Exemple de sortie :
  ```
  actions
  ```
  {: screen}

## Prise en charge des applications volumineuses
{: #large-app-support}

La taille de code maximale d'une action est 48 Mo. Les applications contenant un grand nombre de modules tiers, de bibliothèques natives ou d'outils externes peuvent être exécutées dans le respect de cette limite.

S'il vous arrive de créer une action de package (zip ou jar) de plus de 48 Mo, la solution consiste à étendre l'image d'exécution avec les dépendances, puis d'utiliser un seul fichier source ou une archive de moins de 48 Mo. 

Par exemple, si vous créez un environnement d'exécution Docker personnalisé, incluant les bibliothèques partagées nécessaires, ces dépendances ne sont pas tenues d'être présentes dans le fichier archive. Des fichiers source privés peuvent toujours être intégrés dans l'archive et injectés lors de l'exécution.

La réduction de la taille du fichier archive présente l'autre avantage de diminuer les temps de déploiement. Deux exemples d'environnement d'exécution sont fournis dans les sections suivantes pour illustrer comment réduire les tailles d'application en tirant parti de cette technique.

### Exemple Python

Pour une application Python, voir la procédure suivante utilisée pour réduire la taille de code de l'application.

1. Mettez la bibliothèque `opencv-python` dans opencv.
2. Installez ensuite le binaire opencv dans l'image du système d'exploitation.
3. Vous pouvez ensuite utiliser `requirements.txt` et exécuter `pip install requirements.txt` pour augmenter l'image avec davantage de bibliothèques Python.
4. Vous pouvez ensuite utiliser `action.py` avec la nouvelle image.

### Exemple Node.js

Dans le cadre des efforts mis en oeuvre pour réduire la taille d'une application Node.js, voir la procédure suivante pour installer des packages supplémentaires dans l'image du système d'exploitation :

1. Installez opencv à l'aide de `npm` :
   ```
   npm install opencv
   ```
   {: pre}

2. De même, si vous avez un package `package.json`, installez-le à l'aide de `npm`:
   ```
   npm install package.json
   ```
   {: pre}

3. Utilisez ensuite `action.js` avec la nouvelle image.

## Accès aux métadonnées d'action dans le corps de l'action
{: #accessing-action-metadata-within-the-action-body}

L'environnement de l'action contient plusieurs propriétés spécifiques à l'action en cours d'exécution. Ces propriétés permettent à l'action de fonctionner à l'aide d'un programme avec des actifs OpenWhisk au moyen de l'API REST, ou de définir une alarme interne lorsque l'action est sur le point d'utiliser la totalité du budget de temps qui lui a été alloué. Les propriétés sont accessibles dans l'environnement système de tous les contextes d'exécution pris en charge : actions Node.js, Python, Swift, Java et Docker avec l'utilisation du squelette Docker OpenWhisk.

* `__OW_API_HOST` : hôte d'API pour le déploiement OpenWhisk qui exécute cette action
* `__OW_API_KEY` : clé d'API pour l'objet qui appelle l'action ; peut être une clé d'API restreinte
* `__OW_NAMESPACE` : espace de nom pour l'_activation_ (peut être différent de l'espace de nom pour l'action)
* `__OW_ACTION_NAME` : nom qualifié complet de l'action en cours d'exécution
* `__OW_ACTIVATION_ID` : ID d'activation pour cette instance d'action en cours d'exécution
* `__OW_DEADLINE` : délai approximatif au terme duquel cette action aura consommé la totalité de son quota de durée (mesuré en millisecondes sur l'époque)
