---

copyright:
years: 2017, 2019
lastupdated: "2019-05-15"

keywords: composer, openwhisk, compositions, sequence, branch

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
{:gif: data-image-type='gif'}

# Composer
{: #pkg_composer}

{{site.data.keyword.openwhisk}} prend désormais en charge Composer pour Apache OpenWhisk en tant qu'aperçu technique. Composer étend des séquences Apache OpenWhisk avec des combinateurs supplémentaires ([JS](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md), [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)), qui vous permettent de créer des flux plus complexes incluant le branchement conditionnel, le traitement d'erreurs et des boucles. La documentation complète et les spécifications techniques de Composer sont disponibles dans le [référentiel Git de Composer](https://github.com/apache/incubator-openwhisk-composer).

Vous pouvez également utiliser le projet open source [Kui](https://github.com/ibm/kui) pour faciliter la création, le déploiement et la visualisation de votre code source Composer. Pour plus d'informations sur l'utilisation de Kui avec Composer, voir [Kui sur Github](https://github.com/ibm/kui).
{: note}

## Installation de la bibliothèque Composer pour JavaScript ou Python 3
{: #install_composer}

Vous pouvez utiliser Composer avec des actions écrites dans n'importe quel langage, mais la composition doit être exprimée en JavaScript ou Python 3. Après l'installation, vous pouvez utiliser les commandes Composer `compose/pycompose` et `deploy/pydeploy` pour [configurer et exécuter une composition](#run).
{: shortdesc}

**Pour JavaScript** :
1. Installez le [package Node.js](https://github.com/apache/incubator-openwhisk-composer) de la bibliothèque Composer à l'aide de Node Package Manager.

    ```
        npm install -g openwhisk-composer
    ```
    {: pre}

2.  Confirmez que vous avez installé la bibliothèque en exécutant l'aide sur les commandes Composer.

    ```
    $ compose -h
    $ deploy -h
    ```
    {: codeblock}

    Exemple de sortie :
    ```
    Syntaxe :
        compose composition.js [indicateurs]

    Syntaxe :
        deploy composition composition.json [indicateurs]
    ```
    {: screen}

**Pour Python 3** :
Installez la bibliothèque [Composer pour Python 3](https://github.com/apache/incubator-openwhisk-composer-python) à l'aide de `pip3`.

1.  Clonez le référentiel GitHub Composer pour Python 3.
    ```
    git clone https://github.com/apache/incubator-openwhisk-composer-python.git
    ```
    {: pre}
2.  Accédez au référentiel Composer.
    ```
    cd composer-python
    ```
    {: pre}
3.  Installez la bibliothèque Composer. Incluez le point (`.`) afin que la commande s'exécute dans le répertoire où vous vous trouvez.
    ```
    pip3 install -e .
    ```
    {: pre}
4.  Confirmez que vous avez installé la bibliothèque en exécutant l'aide sur les commandes Composer.
    ```
    $ pycompose -h
    $ pydeploy -h
    ```
    {: codeblock}

    Exemple de sortie :
    ```
    Syntaxe : pycompose composition.py command [indicateurs]
    Syntaxe : pydeploy composition composition.json [indicateurs]
    ```
    {: screen}

## Configuration et exécution de compositions dans IBM Cloud Functions
{: #run}

Vous pouvez utiliser les bibliothèques Composer JavaScript ou Python 3 pour créer vos compositions dans {{site.data.keyword.openwhisk}}. Utilisez `compose` ou `pycompose` pour compiler le code source de votre composition, puis utilisez `deploy` ou `pydeploy` pour déployer la composition dans {{site.data.keyword.openwhisk}}. Après avoir configuré la composition, vous pouvez l'exécuter dans {{site.data.keyword.openwhisk}}.
{: shortdesc}

**Avant de commencer** :
Par défaut, les déploiements utilisent les valeurs définies dans `~/.wskprops`. Remplacez les valeurs par défaut en définissant deux paramètres en entrée pour la commande Composer `deploy` ou `pydeploy`.

1.  Définissez l'hôte d'API sur le noeud final {{site.data.keyword.openwhisk}}.
    ```
    apihost = us-south.functions.cloud.ibm.com
    ```
    {: codeblock}
2.  Ajoutez votre clé d'authentification d'interface de ligne de commande `wsk`. 
    ```
    auth = <wsk-cli-auth-key>
    ```
    {: codeblock}

**Pour exécuter une composition** :

1.  Créez un code source Composer avec les bibliothèque NodeJS ou Python 3. Par exemple, créez un fichier `demo.js`.
2.  Compilez le code source Composer dans un fichier JSON.
    *   Dans JavaScript :
        ```
        compose demo.js > demo.json
        ```
        {: pre}
    *   Dans Python 3 :
        ```
        pycompose demo.js > demo.json
        ```
        {: pre}
3.  Déployez le code dans {{site.data.keyword.openwhisk}}.
    *   Dans JavaScript : Incluez l'indicateur `-w` pour remplacer un déploiement existant nommé `demo`.
        ```
        deploy demo demo.json -w
        ```
        {: pre}
    *   Dans Python 3 : Incluez l'indicateur `-w` pour remplacer un déploiement existant nommé `demo`.
        ```
        pydeploy demo demo.json -w
        ```
        {: pre}
4.  Exécutez la composition en utilisant la même méthode que pour [appeler d'autres actions](/docs/openwhisk?topic=cloud-functions-triggers) dans {{site.data.keyword.openwhisk}}.
    ```
    ibmcloud fn action invoke demo
    ```
    {: pre}

{{site.data.keyword.openwhisk}} exécute le code que vous avez déployé en tant que type d'action spécifique. Pour plus de détails, consultez la documentation sur les [actions du conducteur](https://github.com/apache/incubator-openwhisk/blob/master/docs/conductors.md).

## Extension de séquences avec Composer
{: #extending}

Apache OpenWhisk vous permet de chaîner des fonctions dans une `séquence`, où la sortie d'une action devient l'entrée d'une autre action.

### Séquences sans Composer
{: #sequences-without-composer}
Vous pouvez chaîner deux fonctions appelées `action1` et `action2` dans {{site.data.keyword.openwhisk_short}} :

`ibmcloud fn action create --sequence mysequence action1 action2`.

Le résultat de cette commande est une fonction appelée `mysequence`, qui est composée de `action1` et `action2`.  Vous pouvez utiliser `mysequence` comme toute autre fonction dans OpenWhisk.

### Séquences avec Composer
{: #sequences-with-composer}
Dans Composer, vous pouvez spécifier des séquences plus riches en utilisant le code source plutôt que la ligne de commande.

Pour JavaScript :
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', 'action2')
```
{: codeblock}

Pour Python 3 :
```
import openwhisk-composer

def main():
  return composer.sequence('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-sequence.png" width="35%" title="Séquence simple" alt="Séquence avec deux actions" style="width:250px; border-style: none"/></br>
_Figure 1. Séquence avec deux actions_

Vous n'êtes pas limité au chaînage de fonctions dans Composer. Composer inclut une gamme de combinateurs [JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) ou [Python 3](https://github.com/ibm-functions/composer-python/blob/master/docs/COMBINATORS.md) qui améliorent l'expressivité des séquences. Vous pouvez afficher des exemples courants dans les sections suivantes.

### Traitement d'erreurs
{: #error-handling}
Vous pouvez ajouter le traitement d'erreurs à une séquence en utilisant des blocs `try-catch-finally`. Dans cet exemple, la séquence est entourée du code try. Le code `handleError` s'exécute si l'une des actions renvoie une erreur.

Pour JavaScript :
```
const composer = require('openwhisk-composer')

module.exports = composer.try(
    composer.seq('action1', 'action2'),
    'handleError')
```
{: codeblock}

Pour Python 3 :
```
import openwhisk-composer

def main():
  return composer.do(composer.sequence('action1', 'action2'),
  'handleError')
```
{: codeblock}
</br>
<img src="images/composer-error.png" width="400" title="Séquence try" alt="Séquence avec traitement d'erreurs" style="width:400px; border-style: none"/></br>
_Figure 2. Séquence avec traitement d'erreurs_

### Branchement conditionnel
{: #conditional-branch}
Vous pouvez créer une séquence branchée à l'aide de `if-then-else`. Cet exemple illustre le code `if-then-else`. `action1` doit renvoyer une valeur booléenne. Si la valeur `true` est indiquée, `action2` est exécuté ; sinon, c'est `action3`. Notez que `action3` est facultatif et peut être omis pour `if-then`.

Pour JavaScript :
```
const composer = require('openwhisk-composer')

module.exports = composer.if('action1', 'action2', 'action3')
```
{: codeblock}

Pour Python 3 :
```
import openwhisk-composer

def main():
  return composer.when('action1', 'action2', 'action3')
```
{: codeblock}
</br>
<img src="images/composer-conditional.png" width="250" title="Séquence if" alt="Séquence avec branchement conditionnel" style="width:250px; border-style: none"/></br>
_Figure 3. Séquence avec branchement conditionnel_

### Boucles
{: #loop}
Vous pouvez créer des constructions de bouclage dans Composer. Dans cet exemple, `action2` s'exécute tant que `action1` renvoie la valeur `true`. Composer limite le nombre total d'étapes que vous pouvez exécuter dans une séquence composée. La limite actuelle est 20.

Pour JavaScript :
```
const composer = require('openwhisk-composer')

module.exports = composer.while('action1', 'action2')
```
{: codeblock}

Pour Python 3 :
```
import openwhisk-composer

def main():
  return composer.loop('action1', 'action2')
```
{: codeblock}
</br>
<img src="images/composer-loop.png" width="250" title="Séquence while" alt="Séquence avec boucle while" style="width:250px; border-style: none"/></br>
_Figure 4. Séquence avec boucle `while`_

### Définition d'actions en ligne
{: #inline-def}
Vous pouvez définir des actions dans le code de la composition lui-même. Dans cet exemple, vous allez créer la définition d'action en ligne avec la composition nommée `hello` à l'aide de `composer.action()`.

Pour JavaScript :
```
const composer = require('openwhisk-composer')

module.exports = composer.seq('action1', composer.action('hello', { action: function () { return { message: 'Hello!' } } }))
```
{: codeblock}

Pour Python 3 :
```
import openwhisk-composer

def main():
  return composer.sequence('action1',composer.action('hello', { 'action': "message = 'hello'\ndef main(args):\n    return { 'message':message }" }))
```
{: codeblock}
</br>
<img src="images/composer-inline.png" width="250" title="Séquence while" alt="Séquence avec définition d'actions en ligne" style="width:250px; border-style: none"/></br>
_Figure 5. Séquence avec définition d'actions en ligne_

## Utilisation d'autres définitions de combinateur
{: #combinator-def}
Consultez la documentation pour Composer sur Apache OpenWhisk ([JavaScript](https://github.com/apache/incubator-openwhisk-composer/blob/master/docs/COMBINATORS.md) ou [Python 3](https://github.com/apache/incubator-openwhisk-composer-python/blob/master/docs/COMBINATORS.md)) pour obtenir la liste complète des définitions de combinateurs.

