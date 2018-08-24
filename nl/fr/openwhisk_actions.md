---

copyright:
  years: 2016, 2018
lastupdated: "2018-07-23"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Création et appel d'actions
{: #openwhisk_actions}

Les actions sont des fragments de code sans état qui s'exécutent sur la plateforme {{site.data.keyword.openwhisk}}. Par exemple, une action peut être utilisée pour détecter les visages dans une image, répondre à une modification de base de données, agréger un ensemble d'appels API ou publier un tweet. {:shortdesc}

Les actions peuvent être appelées explicitement ou s'exécuter en réponse à un événement. Dans tous les cas, chaque exécution d'une action génère un enregistrement d'activation identifié par un ID d'activation unique. L'entrée pour une action et le résultat d'une action constituent un dictionnaire de paires clé-valeur, où la clé est une chaîne et la valeur est une valeur JSON valide. Les actions peuvent également être composées d'appels à d'autres actions ou à une séquence définie d'actions.

Une action peut être écrite en tant que fonction JavaScript, Swift, Python, PHP, en tant que méthode Java, ou en tant que exécutable compatible binaire, comme les programmes Go et les exécutables personnalisés sous forme de conteneurs Docker. Apprenez à créer, appeler et déboguer des actions dans votre environnement de développement préféré .

## Création d'actions JavaScript
{: #creating-and-invoking-javascript-actions}

Les sections ci-après expliquent comment utiliser des actions dans JavaScript. Commencez par créer et appeler une action simple. Ensuite, ajoutez des paramètres à l'action et appelez cette action avec des paramètres. Définissez aussi des paramètres par défaut et appelez-les. Enfin, créez des actions asynchrones.

### Création et appel d'une action JavaScript simple
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

2. Créez une action nommée `hello` à l'aide de la fonction JavaScript.
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

4. Exécutez l'action dans le cloud en exécutant un appel de blocage. Les appels de blocage utilisent un style demande/réponse et attendent que le résultat de l'activation soit disponible Le délai d'attente est inférieur à 60 secondes ou à la [limite de temps](./openwhisk_reference.html#openwhisk_syslimits) de l'action.
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
    Pour des conseils sur la surveillance des activations, consultez [Monitoring action output](openwhisk_managing.html#monitor-action-output).
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

### Création d'actions asynchrones
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
    ibmcloud fn action create asyncAction asyncAction.js
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

3. Vérifiez combien de temps a duré l'activation en consultant le journal d'activation.

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

### Utilisation d'actions pour appeler une API externe
{: #openwhisk_apicall_action}

Jusqu'à présent, les exemples fournis sont des fonctions JavaScript autonomes. Vous pouvez également créer une action qui appelle une API externe.
{: shortdesc}

L'exemple suivant appelle le service APOD (Astronomy Picture of the Day) de la NASA qui fournit tous les jours une image unique de notre univers.

1. Sauvegardez le code suivant dans un fichier nommé `apod.js`.
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

    Un appel de l'API de la NASA APOD est émis et les zones sont extraites du résultat au format JSON. Pour plus d'informations sur les packages Node.js que vous pouvez utiliser dans vos actions, voir [System details and limits](./openwhisk_reference.html#openwhisk_ref_javascript_environments).

2. Créez une action nommée `apod`.
    ```
    ibmcloud fn action create apod apod.js
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

### Conditionnement d'une action dans un module Node.js
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
    * Vous devez soit nommer ce fichier **index.js**, soit spécifier le nom de fichier de votre choix via la propriété `main` dans **package.json**.

3. Installez toutes les dépendances en local.
    ```
    npm install
    ```
    {: pre}
    **Remarque **: Alors que la plupart des packages `npm` installent des sources JavaScript avec la commande `npm install`, certains installent et compilent également des artefacts binaires. Actuellement, le téléchargement de fichier archive ne prend pas en charge que les dépendances JavaScript. Les appels d'action risquent d'échouer si l'archive comprend des dépendances binaires.

4. Créez une archive `.zip` contenant tous les fichiers ,notamment toutes les dépendances.
    ```
    zip -r action.zip *
    ```
    {: pre}

    L'utilisation d'une action Windows Explorer pour créer le fichier zip génère une structure incorrecte. Les actions zip {{site.data.keyword.openwhisk_short}} nécessitent que `package.json` soit placé à la racine du fichier zip, alors que l'Explorateur Windows le place dans un dossier imbriqué. L'option la plus sûre consiste à utiliser la commande `zip` de ligne de commande.
    {: tip}

5. Créez l'action. Lorsque vous créez une action depuis une archive `.zip`, vous devez fournir explicitement une valeur pour l'indicateur `--kind` à l'aide de `nodejs:6` ou `nodejs:8`.
    ```
    ibmcloud fn action create packageAction --kind nodejs:6 action.zip
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

### Conditionnement d'une action dans un bundle unique
{: #openwhisk_js_webpack_action}

Si le conditionnement de l'action sous forme de zip inclut trop de fichiers inutiles ou si vous avez besoin d'un déploiement plus rapide, vous pouvez écrire le code minimal dans un seul fichier `.js` qui inclut les dépendances.
{: shortdesc}

Vous pouvez conditionner une action à l'aide d'un programme de mise en bundle de module JavaScript, tel que [webpack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://webpack.js.org/concepts/). Lorsque `webpack` traite votre code, il génère de manière récursive un graphique de dépendance incluant chaque module dont votre action a besoin.

1. Sauvegardez le code suivant dans un fichier nommé `package.json`. `webpack` est ajouté en tant que dépendance de développement.
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

3. Sauvegardez le code suivant dans un fichier nommé `index.js`. La variable `global.main` est définie sur la principale fonction de l'action.
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
        ibmcloud fn action update my-action dist/bundle.js
        ```
        {: pre}

    **Remarque **: Le fichier bundle qui est généré par `webpack` prend en charge uniquement les dépendances JavaScript. Les appels d'action échouent si le bundle dépend de dépendances binaires, car cela n'est pas inclus avec le fichier `bundle.js`.

## Création d'actions Python
{: #creating-python-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Python unique, et comment ajouter des paramètres à cette action.

### Création et appel d'une action Python
{: #openwhisk_actions_python_invoke}

Une action est simplement une fonction Python de premier niveau. Pour créer une action Python :

1. Sauvegardez le code suivant dans un fichier nommé `hello.py`.
    ```python
    def main(args):
        name = args.get("name", "stranger")
        greeting = "Hello " + name + "!"
        print(greeting)
        return {"greeting": greeting}
    ```
    {: codeblock}

  * Les actions Python consomment et produisent toujours un dictionnaire.
  * Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée explicitement pour créer l'action avec l'interface de ligne de commande `wsk` à l'aide de l'indicateur `--main`.

2. Créez une action `helloPython`.
    ```
    ibmcloud fn action create helloPython hello.py
    ```
    {: pre}

    Exemple de sortie :
    ```
    ok: created action helloPython
    ```
    {: screen}

    L'interface de ligne de commande déduit automatiquement le type d'action en utilisant l'extension du fichier source. Pour les fichiers source `.py`, l'action est lancée dans un environnement d'exécution Python 2. Vous pouvez également créer une action opérant avec Python 3 en mentionnant explicitement le paramètre `--kind python:3`. Vous pouvez utiliser également l'environnement d'exécution Python 3 avec le type `python-jessie:3` qui contient des packages supplémentaires pour les services IBM Cloud, tels que {{site.data.keyword.cloudant_short_notm}}, {{site.data.keyword.Db2_on_Cloud_long_notm}}, {{site.data.keyword.cos_full_notm}} et {{site.data.keyword.ibmwatson_notm}}. Pour plus d'informations sur les packages inclus dans cet environnement d'exécution Python 3, voir la section de [référence](./openwhisk_reference.html#openwhisk_ref_python_environments) de l'environnement d'exécution Python.

3. Appelez l'action.
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

Vous pouvez conditionner une action Python et les modules dépendants dans un fichier zip. Par exemple, pour créer une action avec un module auxiliaire appelé `helper.py` :

1. Créez une archive contenant vos fichiers source. **Remarque **: Le fichier source qui contient le point d'entrée doit être nommé `__main__.py`.
    ```bash
    zip -r helloPython.zip __main__.py helper.py
    ```
    {: pre}

2. Créez l'action.
    ```bash
    ibmcloud fn action create helloPython --kind python:3 helloPython.zip
    ```
    {: pre}

### Conditionnement d'actions Python avec un environnement virtuel dans des fichiers zip
{: #openwhisk_actions_python_virtualenv}

Vous pouvez conditionner mes dépendances Python à l'aide d'un environnement virtuel, `virtualenv`. L'environnement virtuel vous permet de lier des packages supplémentaires qui peuvent être installés à l'aide de [`pip` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://packaging.python.org/installing/), par exemple.

Pour installer des dépendances, conditionnez-les dans un environnement virtuel, puis créez une action OpenWhisk compatible :

1. Créez un fichier [requirements.txt ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) contenant les modules `pip` et les versions à installer.

2. Installez les dépendances et créez un environnement virtuel. Le répertoire d'environnement virtuel doit se nommer `virtualenv`. Pour assurer la compatibilité avec le conteneur d'exécution OpenWhisk, les installations de package dans un environnement virtuel doivent utiliser l'image correspondant à ce type.
    - Pour le type `python:2`, utilisez l'image Docker `openwhisk/python2action`.
    - Pour le type `python:3`, utilisez l'image Docker `openwhisk/python3action`.
    - Pour le type `python-jessie:3`, utilisez l'image Docker `ibmfunctions/action-python-v3`.
    ```
    docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3 \
      bash  -c "cd tmp &&virtualenv virtualenv &&source virtualenv/bin/activate &&pip install -r requirements.txt"
    ```
    {: pre}

3. Conditionnez le répertoire `virtualenv` et les éventuels fichiers Python supplémentaires. Le fichier source qui contient le point d'entrée doit être nommé `__main__.py`.
    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}

4. Créez l'action `helloPython`.
    ```
    ibmcloud fn action create helloPython --kind python-jessie:3 helloPython.zip
    ```
    {: pre}

Ajoutez uniquement les modules qui ne font pas partie de l'environnement d'exécution sélectionné dans le fichier `requirements.txt`. Cela permet de conserver une taille minimale pour `virtualenv`.
{: tip}

## Création d'actions PHP
{: #creating-php-actions}

Les sections ci-après vous expliquent comment créer et appeler une action PHP unique, et comment ajouter des paramètres à cette action.

### Création et appel d'une action PHP
{: #openwhisk_actions_php_invoke}

Une action est simplement une fonction PHP de premier niveau. Pour créer une action PHP :

1. Sauvegardez le code suivant dans un fichier nommé `hello.php`.
    ```
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

    * Les actions PHP consomment et renvoient toujours un tableau associatif.
    * Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée explicitement lorsque vous créez l'action avec l'interface de ligne de commande `ibmcloud fn` en utilisant l'indicateur `--main`.

2. Créez une action nommée `helloPHP`.
    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    L'interface CLI déduit automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.php`, l'action est lancée dans un environnement d'exécution PHP 7.1. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_php) au sujet de PHP.

3. Appelez l'action.
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

Vous pouvez conditionner une action PHP, ainsi que pour d'autres fichiers et packages dépendants, dans un fichier zip.
Par exemple, pour conditionner une action avec un second fichier appelé `helper.php` :

1. Créez une archive contenant vos fichiers source. **Remarque **: Le fichier source qui contient le point d'entrée doit être nommé `index.php`.
    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Créez l'action.
    ```bash
    ibmcloud fn action create helloPHP --kind php:7.1 helloPHP.zip
    ```
    {: pre}

## Création d'actions Swift
{: #creating-swift-actions}

Les sections suivantes expliquent comment créer et appeler une action Swift unique et conditionner cette action dans un fichier zip.

**Remarque :** les actions Swift s'exécutent dans un environnement Linux. Swift sous Linux est en cours
de développement et {{site.data.keyword.openwhisk_short}} utilise en principe la dernière édition disponible. Ces éditions ne sont peut-être pas stables. Il se peut que la version de Swift utilisée avec {{site.data.keyword.openwhisk_short}} ne corresponde pas aux versions de Swift provenant d'éditions stables de Xcode sous MacOS.

Pour plus d'informations sur l'environnement d'exécution Swift, consultez la [référence](./openwhisk_reference.html#swift-actions) Swift.
{: tip}

### Création et appel d'une action
{: #openwhisk_actions_swift_invoke}

#### Swift 3
{: #openwhisk_actions_swift3_invoke}

Une action est simplement une fonction Swift de niveau supérieur. Pour créer une action Swift 3 :

1. Sauvegardez le code suivant dans un fichier nommé `hello.swift`.
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

2. Créez une action nommée `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:3.1.1
    ```
    {: pre}

    L'interface CLI déduit automatiquement le type d'action d'après l'extension du fichier source. Pour les fichiers source `.php`, l'action est lancée dans un environnement d'exécution PHP 7.1. Pour plus d'informations, voir le [document de référence](./openwhisk_reference.html#openwhisk_ref_php) au sujet de PHP.

3. Appelez l'action.
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

#### Swift 4
{: #openwhisk_actions_swift4_invoke}

En plus de la signature de la fonction main ci-dessus, Swift 4 fournit deux autres signatures prêtes à l'emploi qui bénéficient du type [Codable ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/swift/codable). Pour en savoir plus sur les types de données codables et décodables permettant la compatibilité avec des représentations externes, cliquez [ici![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

1. Sauvegardez le code suivant dans un fichier nommé `hello.swift`.
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
     L'exemple suivant comporte un paramètre d'entrée en tant qu'entrée codable (`Codable Input`) avec le champ `name`, et un paramètre de sortie codable (`Codable Output`) est renvoyé avec un champ `greetings`.

2. Créez une action nommée `helloSwift`.
    ```
    ibmcloud fn action create helloSwift hello.swift --kind swift:4.1
    ```
    {: pre}

3. Appelez l'action.
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

### Conditionnement d'une action sous forme d'exécutable Swift
{: #packaging-an-action-as-a-swift-executable}

Lorsque vous créez une action Swift {{site.data.keyword.openwhisk_short}} avec un fichier source Swift, celui-ci doit être compilé en fichier binaire avant l'exécution de l'action. Ce délai est dénommé délai de démarrage à froid. Une fois le fichier binaire créé, les appels ultérieurs de l'action sont beaucoup plus rapides jusqu'à ce que le conteneur hébergeant votre action soit purgé. Pour éviter ce délai, vous pouvez compiler votre fichier Swift en binaire, puis télécharger le fichier binaire dans {{site.data.keyword.openwhisk_short}} sous forme de fichier zip. 

Vous pouvez utiliser un script pour automatiser le conditionnement d'une action.

**Prerequis **: Le script utilisé suppose que vous ayez un répertoire appelé `actions`, avec chaque répertoire de niveau supérieur représentant une action.
```
actions/
├── hello
│   ├── Package.swift
│   └── Sources
│       └── main.swift
```

1. Sauvegardez le code suivant dans un fichier script nommé `compile.sh`.
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

2. Pour ajouter des dépendances, créez le fichier `Package.swift`. L'exemple suivant ajoute `example-package-deckofplayingcards` comme dépendance. `CCurl`, `Kitura-net` et `SwiftyJSON` sont fournis dans l'action Swift standard. Par conséquent, vous devez les inclure dans votre propre fichier `Package.swift` uniquement pour les actions Swift 3.
    * Exemple de syntaxe de Swift 3 :
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

    * Exemple de syntaxe de Swift 4 :
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

3. Pour créer un fichier `hello.zip` dans `build`, générez l'action.
    * Swift 3 :
      ```
      bash compile.sh hello swift:3.1.1
      ```
      {: pre}

    * Swift 4 :
      ```
      bash compile.sh hello swift:4.1
      ```
      {: pre}

4. Transférez le zip vers {{site.data.keyword.openwhisk_short}} avec le nom d'action `helloSwiftly`.
    * Swift 3 :
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:3.1.1
      ```
      {: pre}

    * Swift 4 :
      ```
      ibmcloud fn action update helloSwiftly build/hello.zip --kind swift:4.1
      ```
      {: pre}

5. Appelez l'action.
    ```
    ibmcloud fn action invoke helloSwiftly --blocking
    ```
    {: pre}

    La durée d'exécution de l'action figure dans la propriété duration1.

6. Vous pouvez comparer la durée de l'action pré-compilée à la durée d'un appel de commande avec une étape de compilation. Appelez l'action de la section précédente :
    ```
    ibmcloud fn action invoke --result helloSwift --param name World --blocking
    ```
    {: pre}

### Erreur lors du traitement dans Swift 4
{: #error-handling-swift4}

Avec le nouveau gestionnaire completionHandler Codable, vous pouvez transmettre le paramètre error pour indiquer qu'un incident s'est produit dans votre action. Le [traitement des erreurs dans Swift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ressemble au traitement des exceptions dans d'autres langues, avec l'utilisation des mots clés `try`, `catch` et `throw`.

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

Les sections ci-après vous expliquent comment créer et appeler une action Java unique, et comment ajouter des paramètres à cette action.

Pour pouvoir compiler, tester et archiver des fichiers Java, vous devez avoir installé un logiciel [JDK 8 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://openjdk.java.net/install) en local.

### Création et appel d'une action Java
{: #openwhisk_actions_java_invoke}

Une action Java est un programme Java comportant une méthode appelée `main`. `main` doit avoir la signature exacte suivante :
```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}

Pour créer une action Java :

1. Sauvegardez le code suivant dans un fichier nommé `Hello.java`.
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

2. Compilez `Hello.java` dans un fichier JAR nommé `hello.jar`. **Remarque **: [google-gson ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/google/gson) doit exister dans votre CLASSPATH Java.
    ```
    javac Hello.java
    ```
    {: pre}
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}

3. Créez une action.
    ```
    ibmcloud fn action create helloJava hello.jar --main Hello
    ```
    {: pre}
  * Vous devez spécifier le nom de la classe principale à l'aide de `--main`. Une classe principale est éligible si elle implémente une méthode `main` statique. Si la classe ne se trouve pas dans le package par défaut, utilisez le nom de classe qualifié complet Java, par exemple, `--main com.example.MyMain`.
  * Vous pouvez personnaliser le nom de méthode de votre action Java. Pour cela, spécifiez le nom de méthode qualifié complet de votre action, par exemple, `--main com.example.MyMain#methodName`.
  * L'interface CLI déduit automatiquement le type d'action d'après l'extension du fichier source.

4. Appelez l'action.
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
{: shortdesc}

Votre code est compilé dans un fichier binaire exécutable et imbriqué dans une image Docker. Le programme binaire interagit avec le système en prenant l'entrée dans `stdin` et en répondant par le biais de `stdout`.   Des informations sur la création d'actions Docker sont disponibles dans la section [Références](./openwhisk_reference.html#openwhisk_ref_docker).

Prérequis : vous devez disposer d'un compte Docker Hub. Configurez un ID et un compte Docker gratuits sur le site [Docker Hub ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com).

Pour configurer un fichier binaire et utiliser l'image Docker téléchargée en tant qu'action :

1. Téléchargez et installez le squelette Docker. Le squelette est un modèle de conteneur Docker dans lequel vous pouvez injecter votre code sous la forme de fichiers binaires personnalisés.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configurez votre fichier binaire personnalisé dans le squelette Docker. Le squelette inclut un programme C que vous pouvez utiliser. Une partie du fichier `example.c` est compilée dans le cadre du processus de génération de l'image Docker ; par conséquent, il n'est pas nécessaire que C soit compilé sur votre machine. 
  ```
  cat dockerSkeleton/example.c
  ```
  {: pre}

  Exemple de sortie :
  ```c
  #include <stdio.h>
  int main(int argc, char *argv[]) {
      printf("This is an example log message from an arbitrary C program!\n");
      printf("{ \"msg\": \"Hello from arbitrary C program!\", \"args\": %s }",
             (argc == 1) ? "undefined" : argv[1]);
  }
  ```
  {: codeblock}

3. Facultatif : ajoutez du code et des dépendances supplémentaires à l'image Docker en modifiant `Dockerfile` pour générer votre exécutable. Notez les exigences et limites suivantes :
  * Le fichier binaire doit se trouver dans le conteneur dans le répertoire `/action/exec`.
  * Le fichier exécutable reçoit un argument simple depuis la ligne de commande. Cet argument est une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action. 
  * Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  * Par convention, la dernière ligne de la sortie doit être un objet JSON transformé en chaîne qui représente le résultat de l'action.

4. Générez l'image Docker et téléchargez-la à l'aide d'un script fourni.
    1. Connectez-vous à Docker.
        ```
        docker login -u <username> -p <password>
        ```
        {: pre}

    2. Accédez au répertoire `dockerSkeleton`.
        ```
        cd dockerSkeleton
        ```
        {: pre}

    3. Exécutez le script.
        ```
        ./buildAndPush.sh <username>/blackboxdemo
        ```
        {: pre}

5. Utilisez votre conteneur Docker pour créer une action.
    ```
    ibmcloud fn action create example --docker <username>/blackboxdemo
    ```
    {: pre}

6. Appelez l'action.
    ```
    ibmcloud fn action invoke --result example --param payload Rey
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
        "args": {
            "payload": "Rey"
        },
      "msg": "Hello from arbitrary C program!"
  }
    ```
    {: screen}

7. Afin de mettre à jour l'action Docker, télécharger l'image la plus récente dans le dockerhub. Ainsi, le système pourra extraire votre nouvelle image Docker à la prochaine exécution du code pour votre action.
    ```
    ./buildAndPush.sh <username>/blackboxdemo
    ```
    {: pre}

8. S'il existe un conteneur en cours d'exécution utilisant une version précédente de votre image Docker, les nouveaux appels continuent d'utiliser cette image. Mettez à jour l'action de sorte que les nouveaux appels commencent à utiliser la nouvelle image.
    ```
    ibmcloud fn action update example --docker <username>/blackboxdemo
    ```
    {: pre}

9. Facultatif : vous pouvez utiliser l'argument `--native` comme abréviation de `--docker openwhisk/dockerskeleton`. Cet argument simplifie la création et le déploiement d'exécutables qui s'exécutent au sein du SDK d'action Docker standard.
    1. Les étapes précédentes créent un exécutable binaire dans le conteneur situé dans `/action/exec`. Copiez le fichier `/action/exec` sur votre système de fichiers local et compressez-le dans `exec.zip`.
    2. Créez une action Docker qui reçoit l'exécutable sous forme de données d'initialisation. L'argument `--native` remplace l'argument `--docker openwhisk/dockerskeleton`.
        ```
        ibmcloud fn action create example exec.zip --native
        ```
        {: pre}

## Création d'actions Go
{: #creating-go-actions}

L'argument `--native` vous permet de conditionner un exécutable Go sous forme d'action.

Notez les exigences et limites suivantes.
  * Le fichier exécutable Go reçoit un argument simple depuis la ligne de commande. Il s'agit d'une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action. 
  * Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  * Par convention, la dernière ligne de la sortie doit être un objet JSON transformé en chaîne qui représente le résultat de l'action.

Pour créer une action Go :

1. Sauvegardez le code suivant dans un fichier nommé `sample.go`.
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

2. Effectuez une compilation croisée de `sample.go` pour {{site.data.keyword.openwhisk_short}}. L'exécutable doit s'appeler `exec`.
    ```bash
    GOOS=linux GOARCH=amd64 go build -o exec
    zip exec.zip exec
    ibmcloud fn action create helloGo --native exec.zip
    ```
    {: codeblock}

3. Appelez l'action.
    ```bash
    ibmcloud fn action invoke helloGo -r -p name gopher
    {
        "msg": "Hello, gopher!"
    }
    ```
    {: pre}

## Création de séquences d'actions
{: #openwhisk_create_action_sequence}

Vous pouvez créer une action qui assemble des actions dans une séquence. Le résultat d'une action est transmis sous forme d'argument à l'action suivante.
{: shortdesc}

Plusieurs actions d'utilitaire sont fournies dans le package `/whisk.system/utils` que vous pouvez utiliser pour créer votre première séquence. 

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

2. A l'aide des actions `split` et `sort`, créez une séquence d'actions de sorte que le résultat de `split` soit transmis en tant qu'argument à `sort`. Cette séquence d'actions convertit des lignes de texte en tableau et trie les lignes.
  ```
  ibmcloud fn action create sequenceAction --sequence /whisk.system/utils/split,/whisk.system/utils/sort
  ```
  {: pre}

3. Appelez l'action.
    ```
    ibmcloud fn action invoke --result sequenceAction --param payload "Over-ripe sushi,\nThe Master\nIs full of regret."
    ```
    {: pre}

    Dans la sortie, les lignes fractionnées sont triées par ordre alphabétique.
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

**Remarque **:
* Les paramètres transmis entre les actions de la séquence sont explicites, sauf les paramètres par défaut. Par conséquent, les paramètres qui sont transmis à la séquence d'actions ne sont disponibles que pour la première action de la séquence. Le résultat de la première action dans la séquence devient l'objet JSON d'entrée de la deuxième action de la séquence ,et ainsi de suite. Cet objet ne comporte pas les paramètres qui sont transmis initialement à la séquence sauf si la première action les inclut explicitement dans son résultat. Les paramètres d'entrée d'une action sont fusionnés avec ses paramètres par défaut, les premiers étant prioritaires et remplaçant tout paramètre par défaut correspondant. Pour plus d'informations sur l'appel des séquences d'actions avec plusieurs paramètres nommés, voir [Définir les paramètres par défaut sur une action](./parameters.html#default-params-action).
* Une séquence n'a pas de délai d'attente global distinct des délais de chaque action au sein de la séquence. Etant donné qu'une séquence est un pipeline d'opérations, une action qui échoue rompt le pipeline. Si une action dépasse le délai d'attente, la séquence entière est sortie avec cet échec.

## Gestion des actions de grande taille
{: #large-app-support}

La taille de code maximale d'une action est 48 Mo. Les applications contenant un grand nombre de modules tiers, de bibliothèques natives ou d'outils externes peuvent être exécutées dans le respect de cette limite.Si vous créez un package .zip ou .jar de plus de 48 Mo, vous devez étendre l'image d'exécution avec les dépendances, puis d'utiliser un seul fichier source ou une archive de moins de 48 Mo. 

Par exemple, si vous créez un environnement d'exécution Docker personnalisé, incluant les bibliothèques partagées nécessaires, ces dépendances ne sont pas tenues d'être présentes dans le fichier archive. Des fichiers source privés peuvent toujours être intégrés dans l'archive et injectés lors de l'exécution.

### Réduction de la taille des actions
{: #large-app-reduce}

Pour réduire la taille de code d'une application Python :

1. Placez la bibliothèque `opencv-python` dans `opencv`.
2. Installez le binaire opencv dans l'image du système d'exploitation.
3. Augmentez l'image avec davantage de bibliothèques Python en exécutant `pip install requirements.txt`.
4. Utilisez `action.py` avec la nouvelle image.

Pour réduire la taille de code d'une application Node.js :

1. Installez `opencv`.
   ```
   npm install opencv
   ```
   {: pre}

2. Installez `package.json`.
   ```
   npm install package.json
   ```
   {: pre}

3. Utilisez `action.js` avec la nouvelle image.
