---

copyright:
  years: 2018
lastupdated: "2018-05-31"

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:pre: .pre}

# Utilisation des paramètres

Découvrez comment définir les paramètres dans les packages et les actions en vue du déploiement et comment transmettre des paramètres à des actions lors d'un appel. Vous pouvez également utiliser un fichier pour stocker des paramètres et transmettre le nom de fichier dans l'action, au lieu de fournir chaque paramètre individuellement sur la ligne de commande.
{: shortdesc}

Avec les actions sans serveur, les données sont fournies en ajoutant des paramètres dans les actions, qui sont déclarés sous forme d'argument dans la fonction main serverless. Toutes les données arrivent ainsi et les valeurs peuvent être définies de différentes manières. La première option consiste à fournir des paramètres à la création (ou à la mise à jour) d'une action ou d'un package. Cette option est utile pour les données qui restent identiques à chaque exécution, à l'instar des variables d'environnement sur d'autres plateformes, ou pour les valeurs par défaut qui peuvent être remplacées au moment de l'appel. La seconde option consiste à fournir des paramètres lorsque l'action est appelée pour remplacer les paramètres préalablement définis.

## Transmission de paramètres à une action lors de l'appel
{: #pass-params-action}

Il est possible de transmettre des paramètres à une action au moment où elle est appelée. Les exemples fournis utilisent JavaScript mais tous les autres langages fonctionnent de la même manière. Pour voir des exemples détaillés, consultez les rubriques suivantes sur les [actions Javascript](./openwhisk_actions.html#creating-and-invoking-javascript-actions), les [actions Swift](./openwhisk_actions.html#creating-swift-actions), les [actions Python](./openwhisk_actions.html#creating-python-actions), les [actions Java](./openwhisk_actions.html#creating-java-actions), les [actions PHP](./openwhisk_actions.html#creating-php-actions), les [actions Docker](./openwhisk_actions.html#creating-docker-actions) ou les [actions Go](./openwhisk_actions.html#creating-go-actions).

1. Utilisez des paramètres dans l'action. Par exemple, créez un fichier nommé **hello.js** avec le contenu suivant :
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

  Les paramètres d'entrée sont transmis sous forme de paramètre d'objet JSON à la fonction **main**. Remarquez la façon dont les paramètres `name` et `place` sont extraits de l'objet `params` dans cet exemple.

2. Mettez à jour l'action **hello** pour qu'elle soit prête à l'emploi :
  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

  Si vous modifiez vos paramètres de données d'identification qui ne concernent pas un service, l'exécution d'une commande `action update` avec de nouveaux paramètres entraîne la suppression de tous les paramètres existants qui ne sont pas spécifiés dans la commande `action update`. Par exemple, si vous exécutez la commande `action update -p key1 new-value -p key2 new-value` en omettant d'autres paramètres qui étaient définis, ces paramètres n'existeront plus une fois l'action mise à jour. Tous les services qui étaient liés à l'action sont également supprimés. Par conséquent, après avoir mis à jour d'autres paramètres, vous devez à nouveau [lier les services à votre action](./binding_services.html).
  {: tip}

3. Les paramètres peuvent être fournis de manière explicite en utilisant la ligne de commande ou en [fournissant un fichier](./parameters.html#using-parameter-files) qui contient les paramètres désirés.

  Pour transmettre des paramètres directement via la ligne de commande, indiquez une paire clé/valeur pour l'indicateur `--param` :
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
  ```
  {: pre}

  **Réponse :**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Remarquez l'utilisation de l'option `--result` : elle implique un appel bloquant où l'interface de ligne de commande attend la fin de l'activation, puis affiche uniquement le résultat. Pour plus de commodité, cette option peut être utilisée sans `--blocking` qui est automatiquement déduit.

  Par ailleurs, si les valeurs des paramètres spécifiés sur la ligne de commande constituent des structures JSON valides, elles sont analysées et envoyées à votre action en tant qu'objet structuré.

  Par exemple, mettez à jour l'action **hello**, comme suit :
  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
  ```
  {: codeblock}

  L'action attend à présent qu'un paramètre `person` comporte les zones `name` et `place`.

  Ensuite, appelez l'action avec un seul paramètre `person` doté d'une structure JSON valide, comme illustré ci-dessous :
  ```
  ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
  ```
  {: pre}

  **Réponse :**
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Le résultat est identique car l'interface de ligne de commande analyse automatiquement la valeur du paramètre `person` dans l'objet structuré attendu par l'action.

## Définition des paramètres par défaut sur une action
{: #default-params-action}

Les actions peuvent être appelées avec plusieurs paramètres nommés. Souvenez-vous : l'action **hello** de l'exemple précédent attend deux paramètres, le nom d'une personne (*name*) et l'endroit d'où elle vient (*place*).

Plutôt que de transmettre tous les paramètres à une action à chaque fois, vous pouvez lier certains paramètres. L'exemple suivant lie le paramètre *place* pour que l'action prenne par défaut la valeur "Kansas" pour l'endroit :

1. Mettez à jour l'action en utilisant l'option `--param` pour lier des valeurs de paramètre, ou en transmettant un fichier contenant les paramètres dans `--param-file`. Pour obtenir des exemples utilisant des fichiers, consultez la section sur l'[utilisation des fichiers de paramètres](./parameters.html#using-parameter-files).

  Pour indiquer les paramètres par défaut de manière explicite sur la ligne de commande, spécifiez une paire clé/valeur dans l'indicateur `param` :
  ```
  ibmcloud fn action update hello --param place Kansas
  ```
  {: pre}

2. Appelez l'action en ne transmettant cette fois que le paramètre `name`.
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy
  ```
  {: pre}

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}

  Notez que vous n'avez pas eu besoin de spécifier le paramètre place lorsque vous avez appelé l'action. Toutefois, vous pouvez remplacer les paramètres liés en spécifiant la valeur de paramètre au moment de l'appel.

3. Appelez l'action en transmettant les deux valeurs `name` et `place`, et observez le résultat :

  Appelez l'action en utilisant l'indicateur `--param` :
  ```
  ibmcloud fn action invoke --result hello --param name Dorothy --param place "Washington, DC"
  ```
  {: pre}

  Exemple de sortie :
  ```
  {  
      "payload": "Hello, Dorothy from Washington, DC"
  }
  ```
  {: screen}

  Les paramètres définis dans une action au moment de sa création ou de sa mise à jour sont toujours remplacés par un paramètre fourni directement lors de l'appel.
  {: tip}

## Définition des paramètres par défaut sur un package
{: #default-params-package}

Des paramètres peuvent être définis au niveau du package et faire office de paramètres par défaut pour les actions _sauf si_ :

- L'action elle-même comporte un paramètre par défaut.
- L'action comporte un paramètre fourni au moment de l'appel, qui est toujours "prioritaire" lorsque plusieurs paramètres sont disponibles.

L'exemple suivant définit le paramètre par défaut `name` sur le package **MyApp** et illustre une action qui l'utilise.

1. Créez un package avec un paramètre défini :

  ```
  ibmcloud fn package update MyApp --param name World
  ```
  {: pre}

2. Créez une action dans le package **MyApp** :
  ```javascript
     function main(params) {
         return {payload: "Hello, " + params.name};
     }
  ```
  {: codeblock}

  Créez l'action :
  ```
  ibmcloud fn action update MyApp/hello hello.js
  ```
  {: pre}

3. Appelez l'action et examinez le paramètre de package par défaut utilisé :
  ```
  ibmcloud fn action invoke --result MyApp/hello
  ```
  {: pre}

  Exemple de sortie :
  ```
     {
         "payload": "Hello, World"
     }
  ```
  {: screen}

## Utilisation des fichiers de paramètres
{: #using-parameter-files}

Vous pouvez ajouter des paramètres dans un fichier au format JSON, puis transférer ces paramètres en fournissant le nom de fichier avec l'indicateur `--param-file`. Cette méthode peut être utilisée pour la création (ou les mises à jour) du package et de l'action, ainsi que lors de l'appel de l'action.

1. Prenons l'exemple **hello** indiqué précédemment en utilisant `hello.js` avec le contenu suivant :

  ```javascript
  function main(params) {
      return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
  ```
  {: codeblock}

2. Mettez à jour l'action avec le contenu de `hello.js` mis à jour :

  ```
  ibmcloud fn action update hello hello.js
  ```
  {: pre}

3. Créez un fichier de paramètres nommé `parameters.json` contenant des paramètres au format JSON :

  ```json
  {
      "name": "Dorothy",
      "place": "Kansas"
  }
  ```
  {: codeblock}

4. Utilisez le nom de fichier `parameters.json` lorsque vous appelez l'action **hello** et observez le résultat :

  ```
  ibmcloud fn action invoke --result hello --param-file parameters.json
  ```

  Exemple de sortie :
  ```
  {
      "payload": "Hello, Dorothy from Kansas"
  }
  ```
  {: screen}
