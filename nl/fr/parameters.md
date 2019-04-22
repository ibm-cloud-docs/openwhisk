---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-05"

keywords: parameters, passing, invocation, binding

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Utilisation des paramètres

Dans les actions sans serveur, les données sont fournies en ajoutant des paramètres aux actions. Les paramètres sont déclarés sous forme d'argument dans la fonction main serverless.
{: shortdesc}

Vous pouvez fournir des valeurs pour les paramètres de deux manières : 
* **Transmettre des paramètres à une action lors de l'appel** : spécifiez les paramètres lorsque l'action est appelée via des indicateurs d'interface de ligne de commande ou via un fichier. Les paramètres fournis lors de l'appel remplacent les paramètres par défaut qui étaient précédemment définis. 
* **Lier des paramètres à une action ou un package** : définissez les paramètres par défaut lors de la création ou de la mise à jour d'une action ou d'un package. Cette option est utile pour les données qui restent identiques à chaque exécution, à l'instar des variables d'environnement sur d'autres plateformes, ou pour les valeurs par défaut qui peuvent être remplacées au moment de l'appel.

## Transmission de paramètres à une action lors de l'appel
{: #pass-params-action}

Il est possible de transmettre des paramètres à une action au moment où elle est appelée.

1. Sauvegardez le code suivant dans un fichier nommé `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Créez l'action `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Si vous avez utilisé cette action auparavant, vérifiez que l'action ne comporte pas de paramètres définis précédemment en la mettant à jour.
    ```
    ibmcloud fn action update hello hello.js
    ```
    {: pre}

4. Appelez l'action en transmettant les paramètres `name` et `place`.
    ```
    ibmcloud fn action invoke --result hello --param name Dorothy --param place Kansas
    ```
    {: pre}

    **Remarque** : vous pouvez transmettre à la place un fichier de paramètres au format JSON :
    ```
    ibmcloud fn action invoke --result hello --param-file parameters.json
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

5. Vous pouvez également transmettre des paramètres à votre action dans un objet structuré. Par exemple, mettez à jour l'action `hello`, comme suit :
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}

    L'action attend à présent qu'un paramètre `person` comporte les zones `name` et `place`.

6. Appelez l'action avec un seul paramètre `person` qui est un objet JSON valide.
    ```
    ibmcloud fn action invoke --result hello -p person '{"name": "Dorothy", "place": "Kansas"}'
    ```
    {: pre}

    Exemple de sortie :
    ```
    {
        "payload": "Hello, Dorothy from Kansas"
  }
    ```
    {: screen}

## Liaison de paramètres à une action
{: #default-params-action}

Les actions peuvent être appelées avec plusieurs paramètres nommés. Par exemple, l'action de base `hello` attend deux paramètres : le nom d'une personne et son emplacement. 

```javascript
function main(params) {
   return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
```
{: screen}

Plutôt que de transmettre tous les paramètres à une action à chaque fois, vous pouvez lier les paramètres par défaut à l'action. La procédure suivante montre comment lier le paramètre `place` à l'action de base `hello` pour que l'action prenne par défaut la valeur "Kansas" pour l'endroit. 

1. Sauvegardez le code suivant dans un fichier nommé `hello.js`.

    ```javascript
    function main(params) {
       return {payload:  'Hello, ' + params.name + ' from ' + params.place};
  }
    ```
    {: codeblock}

2. Créez l'action `hello`.
    ```
    ibmcloud fn action create hello hello.js
    ```
    {: pre}

3. Mettez à jour l'action pour lier des valeurs de paramètre à l'aide de l'indicateur `--param` et d'une paire clé-valeur. 

    ```
    ibmcloud fn action update hello --param place Kansas
    ```
    {: pre}

    **Remarque** : vous pouvez transmettre à la place un fichier de paramètres au format JSON :
    ```
    ibmcloud fn action update hello --param-file parameters.json
    ```
    {: pre}

    Si vous modifiez vos paramètres de données d'identification qui ne concernent pas un service, l'exécution d'une commande `action update` avec de nouveaux paramètres entraîne la suppression de tous les paramètres existants qui ne sont pas spécifiés dans la commande `action update`. Par exemple, si vous exécutez la commande `action update -p key1 new-value -p key2 new-value` en omettant d'autres paramètres qui étaient définis, ces paramètres n'existeront plus une fois l'action mise à jour. Tous les services qui étaient liés à l'action sont également supprimés. Par conséquent, après avoir mis à jour d'autres paramètres, vous devez à nouveau [lier les services à votre action](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

4. Appelez l'action en ne transmettant que le paramètre `name`.
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

    Comme vous n'avez pas indiqué le paramètre `place` lorsque vous avez appelé l'action, la valeur du paramètre lié par défaut, à savoir `Kansas`, est utilisée. 

5. Vous pouvez remplacer les paramètres liés en spécifiant la valeur de paramètre au moment de l'appel. Appelez l'action en transmettant à la fois les paramètres `name` et `place`.
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

## Liaison de paramètres à un package
{: #default-params-package}

Les paramètres par défaut peuvent également être définis au niveau du package. Les paramètres liés servent de paramètres par défaut pour les actions dans le package, sauf si : 

- L'action elle-même comporte un paramètre par défaut. 
- L'action comporte un paramètre fourni au moment de l'appel. 

L'exemple suivant définit le paramètre par défaut `name` sur le package `MyApp` et illustre une action qui l'utilise.

1. Créez un package, en lui liant le paramètre par défaut `name`.
    ```
    ibmcloud fn package update MyApp --param name World
    ```
    {: pre}

2. Sauvegardez le code suivant dans un fichier nommé `helloworld.js`.

    ```javascript
       function main(params) {
           return {payload: "Hello, " + params.name};
     }
    ```
    {: codeblock}

3. Créez l'action dans le package `MyApp`.
    ```
    ibmcloud fn action update MyApp/hello helloworld.js
    ```
    {: pre}

    Si vous modifiez vos paramètres de données d'identification qui ne concernent pas un service, l'exécution d'une commande `action update` avec de nouveaux paramètres entraîne la suppression de tous les paramètres existants qui ne sont pas spécifiés dans la commande `action update`. Par exemple, si vous exécutez la commande `action update -p key1 new-value -p key2 new-value` en omettant d'autres paramètres qui étaient définis, ces paramètres n'existeront plus une fois l'action mise à jour. Tous les services qui étaient liés à l'action sont également supprimés. Par conséquent, après avoir mis à jour d'autres paramètres, vous devez à nouveau [lier les services à votre action](/docs/openwhisk?topic=cloud-functions-binding_services).
    {: tip}

3. Appelez l'action sans paramètre.
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

    Le paramètre par défaut qui est lié au package est utilisé. 
