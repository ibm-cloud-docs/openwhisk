---

copyright:
  years: 2017, 2019
lastupdated: "2019-05-16"

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
{:gif: data-image-type='gif'}


# Préparation des applications pour les actions
{: #prep}

Que vous transformiez l'une de vos applications pour en faire une application sans serveur ou que vous écriviez un script pour répondre à un événement précis, votre code doit respecter certains critères avant d'être utilisé pour créer une action.
{: shortdesc}

Chaque langage de programmation doit se conformer à des exigences spécifiques pour pouvoir être exécuté, mais les exigences générales suivantes s'appliquent à la plupart d'entre eux :
- Le nom du point d'entrée attendu dans le code est `main` par défaut. Si votre point d'entrée est différent de `main`, un nom personnalisé peut être spécifié lors de la création de l'action, auquel cas, notez bien ce nom.
- Les paramètres d'entrée dans votre application et les résultats de sortie de votre application doivent être formatés selon une structure spécifique pouvant être transmise entre différentes entités. Cette structure dépend du langage de votre code. Par exemple, pour les applications Python, les paramètres doivent être entrés dans votre application sous forme de dictionnaire et le résultat de votre application doit également être structuré sous forme de dictionnaire. Comme vous pouvez également transmettre à votre action des paramètres dans un objet structuré, par exemple un objet JSON, vous pouvez structurer votre code pour attendre un paramètre d'entrée avec des valeurs JSON provenant de certaines zones, comme `name` et `place`.

    Exemple d'entrée JSON :
    ```json
    {"name": "Dorothy", "place": "Kansas"}
    ```
    {: codeblock}

    Exemple JavaScript :
    ```javascript
    function main(params) {
        return {payload:  'Hello, ' + params.person.name + ' from ' + params.person.place};
  }
    ```
    {: codeblock}
- Si votre application contient plusieurs fichiers, ils doivent être regroupés en un seul fichier pour être utilisés dans une action. Vous pouvez soit réécrire votre code dans un fichier ou assembler les fichiers et dépendances dans un seul fichier archive. Si votre environnement d'exécution n'est pas pris en charge, vous pouvez intégrer votre application dans une image Docker.
- Les dépendances doivent également être regroupées avec votre application. Les environnements d'exécution disponibles sont fournis avec des extensions et des packages préinstallés. [Examinez les informations de référence correspondant à votre environnement d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes) pour voir si une dépendance de votre application est déjà incluse avec l'environnement d'exécution. Dans l'affirmative, vous n'avez pas besoin de l'inclure dans le package contenant votre application.

    La compilation du code n'est pas une obligation. Cependant et si votre environnement d'exécution le permet, la compilation préalable de votre code peut améliorer les performances.
    {: tip}

## Préparation des applications dans des images Docker
{: #prep_docker}

Avec {{site.data.keyword.openwhisk_short}}, vous pouvez écrire votre application dans le langage de votre choix et la packager sous forme d'image Docker.
{: shortdesc}

Vous ne pouvez utiliser que des images en provenance de registres publics, par exemple une image accessible au public sur Docker Hub. Les registres privés ne sont pas pris en charge.
{: important}

### Package de code dans des images Docker
{: #prep_docker_pkg}

Votre code est compilé dans un fichier binaire exécutable et incorporé dans une image Docker. Le programme binaire interagit avec le système en prenant l'entrée dans `stdin` et en répondant par le biais de `stdout`.
{: shortdesc}

Avant de commencer :
- Vous devez disposer d'un compte Docker Hub. Vous pouvez configurer un ID et un compte Docker gratuits sur le site [Docker Hub ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://hub.docker.com).
- [Installez Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Examinez les conditions requises pour l'environnement d'exécution Docker](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_docker).

Pour packager votre application :

Pour intégrer votre code sous forme d'image Docker :
1. Téléchargez et installez le squelette Docker. Le squelette est un modèle de conteneur Docker dans lequel vous pouvez injecter votre code sous la forme de fichiers binaires personnalisés.
  ```
  ibmcloud fn sdk install docker
  ```
  {: pre}

2. Configurez votre fichier binaire personnalisé dans le squelette de boîte noire. Le squelette inclut un programme C que vous pouvez utiliser. Une partie du fichier `example.c` est compilée dans le cadre du processus de génération de l'image Docker ; par conséquent, il n'est pas nécessaire que C soit compilé sur votre machine.
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

3. Facultatif : ajoutez du code et des dépendances supplémentaires à l'image Docker en modifiant `Dockerfile` pour générer votre exécutable. Notez les configurations requises suivantes :
  * Le fichier binaire doit se trouver dans le conteneur dans le répertoire `/action/exec`.
  * Le fichier exécutable reçoit un argument simple depuis la ligne de commande. Cet argument est une sérialisation de chaîne de l'objet JSON représentant les arguments de l'action.
  * Le programme peut effectuer la journalisation dans `stdout` ou `stderr`.
  * Par convention, la dernière ligne de la sortie doit être un objet JSON transformé en chaîne qui représente le résultat de l'action.
  Pour plus d'informations sur la construction des Dockerfiles, voir le document [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

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




## Préparation des applications JavaScript
{: #prep_js}

Avant de créer une action, préparez votre code JavaScript. Vérifiez que votre code est structuré correctement, puis déterminez s'il doit être inclus dans un package.
{: shortdesc}

### Structuration de code JavaScript
{: #prep_js_struct}

- Le nom attendu pour la fonction de point d'entrée est `main`. Si la fonction dans votre code est différente de `main`, notez le nom pour l'indiquer lors de la création de l'action.
- Les paramètres d'entrée sont transmis sous forme d'objet JSON.
- Le résultat d'une activation réussie est également un objet JSON mais il est renvoyé différemment suivant si l'action est [synchrone](#prep_js_sync) ou [asynchrone](#prep_js_async).



Exemple :
```javascript
  function main() {
      return {payload: 'Hello world'};
  }
  ```

Exemple avec des fonctions multiples :

  ```javascript
  function main() {
      return { payload: helper() }
  }

  function helper() {
      return new Date();
}
  ```
  {: codeblock}


### Structuration de code JavaScript avec un comportement synchrone
{: #prep_js_sync}

L'activation JavaScript est synchrone lorsque la fonction main existe sans exécuter une instruction `return` ou quitte en exécutant une instruction `return` qui renvoie n'importe quelle valeur à l'exception d'un objet Promise.
{: shortdesc}

Exemple de code synchrone :

```javascript
// each path results in a synchronous activation
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




### Structuration de code JavaScript avec un comportement asynchrone
{: #prep_js_async}

Les fonctions JavaScript peuvent poursuivre leur exécution dans une fonction de rappel même après l'instruction return. L'activation JavaScript est asynchrone si la fonction main quitte en renvoyant une promesse (objet Promise). Dans ce cas, le système considère que l'action est toujours en cours d'exécution jusqu'à ce que la promesse soit réalisée ou rejetée. Les fonctions JavaScript qui s'exécutent en mode asynchrone peuvent renvoyer le résultat de l'activation au retour de la fonction `main` en renvoyant une promesse dans votre action.
{: shortdesc}

Commencez par instancier une nouvel objet Promise et par transmettre une fonction de rappel. La fonction de rappel admet deux arguments, resolve et reject, qui sont tous les deux des fonctions. Tout le code asynchrone est inséré dans ce rappel. Le gestionnaire d'action proprement dit peut avoir n'importe quel nom, tant que celui-ci est conforme à la signature habituelle pour l'acceptation et le renvoi d'un objet (ou d'une promesse (`Promise`) d'objet).

Dans l'exemple suivant, vous pouvez voir comment réaliser une promesse en appelant la fonction de résolution (resolve).
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

Cet exemple montre comment rejeter une promesse en appelant la fonction de rejet (reject).
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

Dans les exemples ci-dessus, les détails suivants sont exécutés.
* La fonction `main` renvoie une promesse (Promise). La promesse indique que l'activation n'est pas encore terminée mais qu'on doit s'attendre à ce qu'elle le soit ultérieurement.
* La fonction JavaScript `setTimeout()` attend 2 secondes avant d'appeler la fonction de rappel de la promesse, ce qui représente le code asynchrone.
* Le rappel de la promesse admet les arguments `resolve` et `reject`, qui constituent tous les deux des fonctions.
  * L'appel à la méthode `resolve()` réalise la promesse et indique que l'activation s'est terminée normalement.
  * Un appel à la méthode `reject()` peut être utilisé pour rejeter la promesse et signaler que l'activation ne s'est pas terminée normalement.


### Structuration de code JavaScript avec un comportement synchrone et asynchrone
{: #prep_js_both}

Une action peut être synchrone sur certaines entrées et asynchrone sur d'autres, comme indiqué dans l'exemple suivant.
{: shortdesc}

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





### Exemple : Appel d'une API externe avec JavaScript
{: #prep_js_api}

L'exemple suivant appelle l'API externe du service APOD (Astronomy Picture of the Day) de la NASA, qui fournit tous les jours une image unique de notre univers.
{: shortdesc}


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

Un appel de l'API de la NASA APOD est émis et les zones sont extraites du résultat au format JSON.

Ensuite, [créez](/docs/openwhisk?topic=cloud-functions-actions) et [appelez l'action](/docs/openwhisk?topic=cloud-functions-test) pour la tester. L'exemple d'objet suivant est renvoyé :

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






### Package de code Javascript avec le module webpack
{: #prep_js_pkg}

Vous pouvez intégrer une application dans un package en utilisant un programme de mise en bundle de module JavaScript, tel que [webpack ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://webpack.js.org/concepts/). Lorsque `webpack` traite votre code, il génère de manière récursive un graphique de dépendance incluant chaque module dont votre action a besoin.
{: shortdesc}

Avant de commencer, [examinez les packages inclus avec l'environnement d'exécution JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) pour voir si une dépendance de votre application est déjà comprise avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

1. Créez un fichier `package.json`. Ajoutez `webpack` en tant que dépendance de développement.

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

3. Préparez votre code d'application. Dans cet exemple, nommé `index.js`, la variable `global.main` est définie avec la fonction main de l'application.

    Exemple :
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

    Le fichier bundle qui est généré par `webpack` prend en charge uniquement les dépendances JavaScript. Les appels d'action risquent d'échouer si le bundle dépend de dépendances de fichiers binaires, car cela n'est pas inclus avec le fichier `bundle.js`.
    {: tip}



### Package de code JavaScript sous forme de fichiers NPM
{: #prep_js_npm}

En guise d'alternative à l'écriture de tout votre code d'action dans un seul fichier source JavaScript, vous pouvez inclure votre code sous forme de package `npm` dans un fichier .zip.
{: shortdesc}

Avant de commencer, [examinez les packages inclus avec l'environnement d'exécution JavaScript](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_javascript_environments) pour voir si une dépendance de votre application est déjà comprise avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

1. Dans le répertoire racine, créez un fichier `package.json`. Exemple :

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

2. Installez toutes les dépendances en local.

    ```
    npm install DEPENDENCY
    ```
    {: pre}

    **Remarque **: Alors que la plupart des packages `npm` installent des sources JavaScript avec la commande `npm install`, certains installent et compilent également des artefacts binaires. Le téléchargement de fichier archive prend en charge uniquement les dépendances JavaScript. Si l'archive contient des dépendances binaires, les appels d'action risquent de ne pas aboutir.

3. Créez une archive `.zip` contenant tous les fichiers, notamment toutes les dépendances.

    ```
    zip -r action.zip *
    ```
    {: pre}

    Utilisateurs Windows : l'utilisation de l'action de l'Explorateur Windows pour créer le fichier .zip renvoie une structure de fichier incorrecte. Les actions .zip de {{site.data.keyword.openwhisk_short}} doivent avoir le fichier `package.json` à la racine de l'archive, mais l'Explorateur Windows les place dans un dossier imbriqué. Utilisez à la place la commande `zip`.
    {: tip}





## Préparation des applications Go
{: #prep_go}

Utilisez un fichier unique à des fins de test rapide ou de développement. Pour les applications de production, précompilez vos actions Go dans un exécutable pour améliorer les performances ou prendre en charge plusieurs fichiers source, notamment des bibliothèques fournisseur.
{: shortdesc}

Bien que vous puissiez créer un fichier binaire sur une plateforme Go en effectuant une compilation croisée avec `GOOS=Linux` et `GOARCH=amd64`, utilisez la fonction de précompilation qui est incorporée dans l'image du conteneur d'exécution. Vous pouvez constituer un package avec [plusieurs fichiers source](#prep_go_multi) ou [bibliothèques fournisseur](#prep_go_vendor).
{: tip}


### Structuration de code Go
{: #prep_go_struct}

- Le nom attendu pour le package de point d'entrée est `main`. Si le package dans votre code est différent de `main`, notez le nom pour l'indiquer lors de la création de l'action.
- Le package doit être public.

Exemple :
```go
    package main

    import "fmt"

    // Main is the function implementing the action
    func Main(params map[string]interface{}) map[string]interface{} {
        // parse the input JSON
        name, ok := params["name"].(string)
        if !ok {
            name = "World"
        }
        msg := make(map[string]interface{})
        msg["body"] = "Hello " + name + "!"
        // can optionally log to stdout (or stderr)
        fmt.Println("hello Go action")
        // return the output JSON
        return msg
    }
    ```
    {: codeblock}

### Package de plusieurs fichiers source GO
{: #prep_go_multi}

1. Créez un répertoire `src` de niveau supérieur. Placez les fichiers source appartenant au package principal à la racine de `src` ou dans un répertoire `main` et créez des sous-répertoires pour les autres packages. Par exemple, le package `hello` devient le répertoire `src/hello`.
  ```
  go-action-hello/
  └── src
      ├── hello
      │   └── hello.go
      └── main
          └── main.go
  ```
  {: screen}

2. Importez les sous-packages. Exemple d'importation du sous-package hello par `main/main.go` :

  ```go
  package main

  import (
  	"fmt"
  	"hello"
  )

  // Main forwading to Hello
func Main(args map[string]interface{}) map[string]interface{} {
  	fmt.Println("This is main.Main")
	greetings := "World"
	name, ok := args["name"].(string)
	if ok {
  		greetings = name
  	}
  	return hello.Hello(greetings)
  }
  ```
  {: codeblock}

  Exemple de `hello/hello.go` :

  ```go
  package hello

  import "fmt"

  // Hello return a greeting message
func Hello(name string) map[string]interface{} {
  	fmt.Println("This is hello.Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello " + name
	return res
}
  ```
  {: codeblock}

3. Compilez le code. Créez une archive .zip du répertoire `src`. **N'incluez pas** le répertoire de projet de niveau supérieur `go-action-project/`.

  ```bash
  cd src
  zip -r ../hello-src.zip *
  cd ..
  ```
  {: pre}

  Vous pouvez compiler localement en définissant la variable `GOPATH` sur le parent du répertoire `src`. Si vous utilisez VSCode, vous devez remplacer la valeur de `go.inferGopath` par `true`.
  {: note}

4. Compilez et packagez l'exécutable Go sous la forme `exec` dans la racine de l'archive .zip. Générez l'archive `hello-bin.zip` en exécutant la commande ci-dessous. L'interface de ligne de commande Docker doit être installée sur votre poste de travail et vous devez avoir `docker` dans votre variable `PATH`. 

  ```bash
  docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
  ```
  {: pre}

  Dans cet exemple, la fonction main est `-compile main`. Pour utiliser une autre fonction en tant que fonction main, modifiez la valeur de `-compile`. La fonction main est sélectionnée au moment de la compilation. Lors de la précompilation, `ibmcloud fn action [update | create]` ignore le paramètre `--main`.

  Le conteneur extrait le contenu du fichier source .zip dans `stdin`, le compile et crée une nouvelle archive .zip avec l'exécutable `exec` dans la racine. Le contenu de l'archive .zip est transféré vers `stdout`, qui est redirigé vers l'archive `hello-bin.zip` à déployer en tant qu'action Go.




### Package de code Go avec des bibliothèques fournisseur
{: #prep_go_vendor}

Vous pouvez inclure des dépendances en remplissant un répertoire `vendor` dans l'archive `zip` source lorsque vous compilez les fichiers Go. Le répertoire `vendor` ne fonctionne pas au niveau supérieur. Vous devez placer le répertoire `vendor` dans `src/` à l'intérieur d'un répertoire de package.
{: shortdesc}

Exemple de package de journaux `logrus` dans une application `hello.go` :

```go
package hello

import (
	"os"

	"github.com/Sirupsen/logrus"
)

var log = logrus.New()

func init() {
	log.Out = os.Stdout
}

// Hello return a greeting message
func Hello(name string) map[string]interface{} {
	log.WithFields(logrus.Fields{"greetings": name}).Info("Hello")
	res := make(map[string]interface{})
	res["body"] = "Hello, " + name
	return res
}
```
{: codeblock}

</br>
Dans cet exemple, le répertoire `vendor` se trouve dans `src/hello/vendor`. Vous pouvez ajouter des bibliothèques tierces qui sont utilisées par le package `hello`. Vous pouvez utiliser plusieurs outils tels que [dep ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://golang.github.io/dep/docs/installation.html) pour alimenter et gérer les dépendances.

Vous pouvez utiliser `dep` en créant un fichier `src/main/Gopkg.toml` qui décrit la version et l'emplacement des bibliothèques.

```toml
[[override]]
  name = "github.com/sirupsen/logrus"
  version = "1.1.1"
```
{: codeblock}

Alimentez le répertoire `vendor` et exécutez `dep ensure`.







## Préparation des applications Swift
{: #prep_swift}

Les fichiers Swift doivent être compilés dans un fichier binaire avant l'exécution d'une action. Ce délai est dénommé délai de démarrage à froid. Pour éviter ce délai, vous pouvez compiler votre fichier Swift en binaire, puis télécharger le fichier binaire dans {{site.data.keyword.openwhisk_short}} sous forme de fichier .zip. L'environnement d'exécution Docker inclut un compilateur pour aider les utilisateurs à compiler et packager des actions Swift 4.2. Les appels ultérieurs de l'action sont beaucoup plus rapides jusqu'à ce que le conteneur hébergeant votre action soit purgé. 

Les actions Swift s'exécutent dans un environnement Linux. Swift sous Linux est en cours de développement et {{site.data.keyword.openwhisk_short}} utilise en principe la dernière édition disponible. Ces éditions ne sont peut-être pas stables. Il se peut que la version de Swift utilisée avec {{site.data.keyword.openwhisk_short}} ne corresponde pas aux versions de Swift provenant d'éditions stables de Xcode sous MacOS.
{: important}



### Structuration de code Swift
{: #prep_swift_struc}

Le nom attendu pour la fonction de point d'entrée est `main`. Si la fonction dans votre code est différente de `main`, notez le nom pour l'indiquer lors de la création de l'action.

En plus de la signature de la fonction main ci-dessus, Swift 4 fournit deux autres signatures prêtes à l'emploi qui bénéficient du type [Codable ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/swift/codable). Vous pouvez obtenir plus d'informations sur les types de données [codables et décodables permettant la compatibilité avec des représentations externes comme JSON ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/documentation/foundation/archives_and_serialization/encoding_and_decoding_custom_types).

Exemple :
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


#### Traitement des erreurs dans Swift
{: #prep_swift_error}

Avec le nouveau gestionnaire completionHandler Codable, vous pouvez transmettre le paramètre error pour indiquer qu'un incident s'est produit dans votre action. Le [traitement des erreurs dans Swift ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html) ressemble au traitement des exceptions dans d'autres langages, avec l'utilisation des mots clés `try`, `catch` et `throw`.
{: shortdesc}

Le fragment suivant montre un exemple de traitement d'erreur.

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


### Package de fichier Swift 4.2 dans un fichier binaire
{: #prep_swift42_single}

Compilez un fichier source unique qui ne dépend pas de bibliothèques externes. Utilisez l'indicateur `-compile` avec le nom de la méthode main.

Avant de commencer :
- [Installez Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Examinez les packages inclus dans l'environnement d'exécution Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) pour voir si une dépendance de votre application est déjà incluse avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

Pour packager votre application :

```bash
docker run -i openwhisk/action-swift-v4.2 -compile main <hello.swift >hello.zip
```
{: pre}

Le conteneur Docker lit le contenu du fichier à partir de `stdin` et écrit une archive .zip avec l'exécutable swift compilé dans `stdout`.



### Package de projets Swift 4.2 à fichiers multiples et de dépendances
{: #prep_swift42_multi}

Avant de commencer :
- [Installez Docker](https://hub.docker.com/search?offering=community&type=edition).
- [Examinez les packages inclus dans l'environnement d'exécution Swift](/docs/openwhisk?topic=cloud-functions-runtimes#swift-actions) pour voir si une dépendance de votre application est déjà incluse avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

Pour packager votre application :

1. Pour compiler plusieurs fichiers et inclure des dépendances externes, créez la structure de répertoire suivante.

  ```
  .
  ├── Package.swift
  └── Sources
      └── main.swift
  ```
  {: codeblock}

  Le répertoire `Sources/` contient un fichier nommé `main.swift`.

  Le fichier `Package.swift` doit commencer par un commentaire indiquant la version `4.2` pour les outils Swift :

  ```swift
  // swift-tools-version:4.2
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
      .package(url: "https://github.com/IBM-Swift/SwiftyRequest.git", .upToNextMajor(from: "1.0.0"))
      ],
            targets: [
      .target(
        name: "Action",
        dependencies: ["SwiftyRequest"],
        path: "."
    )
      ]
  )
  ```
  {: codeblock}

2. Créez une archive .zip avec le contenu du répertoire :

  ```bash
  zip ../action-src.zip -r *
  ```
  {: codeblock}

3. Transmettez l'archive .zip au conteneur Docker via `stdin`. Le fichier `stdout` est une nouvelle archive .zip avec l'exécutable compilé. Le conteneur Docker lit le contenu de l'archive .zip à partir de `stdin` et écrit une nouvelle archive .zip avec l'exécutable Swift compilé dans `stdout`.

  ```
  docker run -i openwhisk/action-swift-v4.2 -compile main <action-src.zip >../action-bin.zip
  ```
  {: codeblock}

  Dans un système Linux, vous pouvez combiner les étapes `zip` et `docker run` dans une commande unique : 

  ```
  zip - -r * | docker run -i openwhisk/action-swift-v4.2 -compile main >../action-bin.zip
  ```
  {: codeblock}





## Package d'applications Python
{: #prep_python}


### Structuration de code Python
{: #prep_python_struct}

- Les applications Python doivent consommer et produire un dictionnaire.
- Le nom attendu pour la méthode de point d'entrée est `main`. Si la fonction dans votre code est différente de `main`, notez le nom pour l'indiquer lors de la création de l'action.
{: shortdesc}

Exemple :
```python
def main(args):
	name = args.get("name", "stranger")
	greeting = "Hello " + name + "!"
	print(greeting)
        return {"greeting": greeting}
```

### Package de code Python
{: #prep_python_pkg}

Intégrez le code Python et les modules dépendants dans un fichier .zip. Dans cet exemple, le fichier source qui contient le point d'entrée est `__main__.py` et les modules auxiliaires se trouvent dans un fichier nommé `helper.py`.

Avant de commencer, [examinez les packages inclus avec l'environnement d'exécution Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) pour voir si une dépendance de votre application est déjà comprise avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

Pour packager votre application :

```bash
zip -r helloPython.zip __main__.py helper.py
```
{: pre}


### Package de code Python avec un environnement virtuel dans des fichiers .zip
{: #prep_python_virtenv}

Vous pouvez packager les dépendances Python à l'aide d'un environnement virtuel, `virtualenv`. L'environnement virtuel vous permet de lier des packages supplémentaires qui peuvent être installés à l'aide de [`pip` ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://packaging.python.org/installing/).

Avant de commencer, [examinez les packages inclus avec l'environnement d'exécution Python](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) pour voir si une dépendance de votre application est déjà comprise avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

Pour packager votre application :

1. Créez un fichier [requirements.txt ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://pip.pypa.io/en/latest/user_guide/#requirements-files) contenant les modules `pip` et les versions à installer.

  Pour que `virtualenv` conserve une taille minimale, ajoutez uniquement les modules qui ne font pas partie de l'environnement d'exécution sélectionné dans le fichier `requirements.txt`. Pour plus d'informations sur les packages figurant dans les environnements d'exécution, voir le document de [référence de l'environnement d'exécution](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_python_environments) Python.
  {: tip}

2. Extrayez l'une des images suivantes pour votre environnement d'exécution. Pour assurer la compatibilité avec le conteneur d'exécution, les installations intégrées à des packages dans un environnement virtuel doivent utiliser l'image correspondant à l'environnement d'exécution spécifié.
    * Pour `python:3.7`, utilisez l'image Docker `ibmfunctions/action-python-v3.7`.
    * Pour `python:3.6`, utilisez l'image Docker `ibmfunctions/action-python-v3.6`.
    * Pour `python:2`, utilisez l'image Docker `openwhisk/python2action`.

   Exemple :
   ```
   docker pull ibmfunctions/action-python-v3.7
   ```
   {: pre}

2. Installez les dépendances et créez un environnement virtuel. Le répertoire d'environnement virtuel doit se nommer `virtualenv`.

   ```
   docker run --rm -v "$PWD:/tmp" ibmfunctions/action-python-v3.7 bash -c "cd /tmp && virtualenv virtualenv && source virtualenv/bin/activate && pip install -r requirements.txt"
   ```
   {: pre}

3. Créez un package comprenant le répertoire `virtualenv` et les éventuels fichiers Python supplémentaires. Le fichier source qui contient le point d'entrée doit être nommé `__main__.py`.

    ```
    zip -r helloPython.zip virtualenv __main__.py
    ```
    {: pre}





## Préparation des applications Ruby
{: #prep_ruby}

Avant de créer une action, préparez votre code Ruby.

### Structuration de code Ruby
{: #prep_ruby_struct}

* Les actions Ruby utilisent et renvoient toujours un hachage (collection semblable à un dictionnaire).
* Le nom attendu pour la fonction de point d'entrée est `main`. Si la fonction dans votre code est différente de `main`, notez le nom pour l'indiquer lors de la création de l'action.


Exemple :
```ruby
    def main(args)
      name = args["name"] || "stranger"
      greeting = "Hello #{name}!"
      puts greeting
      { "greeting" => greeting }
    end
    ```
    {: codeblock}

### Package de code Ruby
{: #prep_ruby_pkg}

Vous pouvez intégrer une application Ruby et les packages dépendants dans un fichier .zip. Par exemple, vous pouvez intégrer un second fichier appelé `helper.rb` dans le package de l'action.

Créez une archive contenant vos fichiers source. Le fichier source qui contient le point d'entrée doit être nommé `main.rb`.

```bash
zip -r helloRuby.zip main.rb helper.rb
```
{: pre}

Les modules GEM `mechanize` et `jwt` sont disponibles en plus des modules GEM regroupés et par défaut. Vous pouvez utiliser des modules GEM arbitraires tant que vous utilisez des actions compressées pour intégrer toutes les dépendances.



## Préparation des applications PHP
{: #prep_php}

Avant de créer une action, préparez votre code PHP.

### Structuration de code PHP
{: #prep_php_struct}

- Les actions PHP consomment et renvoient toujours un tableau associatif.
- Le nom attendu pour la fonction de point d'entrée est `main`. Si la fonction dans votre code est différente de `main`, notez le nom pour l'indiquer lors de la création de l'action.

Exemple :
```php
<?php
function main(array $args) : array
{
    $name = $args["name"] ?? "stranger";
    $greeting = "Hello $name!";
    echo $greeting;
    return ["greeting" => $greeting];
}
?>
```
{: codeblock}

### Package de code PHP
{: #prep_php_pkg}

Vous pouvez intégrer des fichiers PHP ou des packages dépendants dans un fichier .zip.

Avant de commencer, [examinez les packages inclus avec l'environnement d'exécution PHP](/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php) pour voir si une dépendance de votre application est déjà comprise avec l'environnement d'exécution. Si votre dépendance n'est pas incluse, vous devez l'intégrer dans le package contenant votre application.

Pour packager votre application :

```bash
zip -r ARCHIVE_NAME.zip FILE_1.php FILE_2.php
```
{: pre}

Exemple :
```bash
zip -r helloPHP.zip index.php helper.php
```
{: pre}







## Préparation des applications Java
{: #prep_java}

Avant de créer une action, préparez votre code Java.

### Structuration de code Java
{: #prep_java_struct}

Une action Java est un programme Java comportant une méthode appelée `main`. `main` doit avoir la signature suivante.


```java
public static com.google.gson.JsonObject main(com.google.gson.JsonObject);
```
{: codeblock}


* Vous devez spécifier le nom de la classe main à l'aide de `--main`. Une classe main est éligible si elle implémente une méthode `main` statique. Si la classe ne se trouve pas dans le package par défaut, utilisez le nom de classe qualifié complet Java, par exemple, `--main com.example.MyMain`.
* Vous pouvez personnaliser le nom de méthode de votre action Java. Pour cela, spécifiez le nom de méthode qualifié complet de votre action, par exemple, `--main com.example.MyMain#methodName`.
* Le type d'action est déterminé à l'aide de l'extension du fichier source.

Exemple :
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


### Package de code Java
{: #prep_java_pkg}

Pour compiler, tester et archiver des fichiers Java, vous devez avoir un logiciel [JDK 8 ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](http://openjdk.java.net/install) installé en local.

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

2. Compilez le fichier `Hello.java` dans un fichier classe.

    ```
    javac Hello.java
    ```
    {: pre}

2. Compressez le fichier classe dans un fichier JAR nommé `hello.jar`. **Remarque **: [google-gson ![Icône de lien externe](../icons/launch-glyph.svg "Icône de lien externe")](https://github.com/google/gson) doit exister dans votre CLASSPATH Java.
3.
    ```
    jar cvf hello.jar Hello.class
    ```
    {: pre}


### Package de code Java avec Gradle
{: #prep_java_gradle}

Vous pouvez utiliser un outil de génération tel que [Gradle](https://gradle.org) pour extraire les bibliothèques d'un référentiel comme Maven Central et générer une archive JAR finale incluant votre code et toutes les dépendances.

Voici un exemple d'utilisation de Gradle pour générer une action Java exploitant la bibliothèque `com.google.zxing` qui permet de générer une image de code QR.

1. Créez un fichier nommé `build.gradle` et spécifiez les dépendances.

  ```gradle
  apply plugin: 'java'

  version = '1.0'

  repositories {
     mavenCentral()
  }

  configurations {
      provided
      compile.extendsFrom provided
  }

  dependencies {
       provided 'com.google.code.gson:gson:2.6.2'
      compile 'com.google.zxing:core:3.3.0'
       compile 'com.google.zxing:javase:3.3.0'
  }

  jar {
      dependsOn configurations.runtime

     from {
          (configurations.runtime - configurations.provided).collect {
              it.isDirectory() ? it : zipTree(it)
          }
      }
  }
  ```
  {: codeblock}

2. Exécutez la commande `gradle jar`, qui génère une archive JAR dans le répertoire `build/libs/`.

  Pour plus d'informations, lisez la documentation Gradle [Declaring Dependencies](https://docs.gradle.org/current/userguide/declaring_dependencies.html#declaring_dependencies).





## Préparation des applications .NET Core
{: #prep_dotnet}

Avant de créer une action, préparez votre code .NET Core.

### Structuration de code .NET
{: #prep_dotnet_struct}

Une action .NET Core est une bibliothèque de classe .NET Core avec une méthode qui est censée être nommée `Main`. Si la méthode dans votre code est différente de `Main`, notez le nom pour l'indiquer lors de la création de l'action au format suivant : `--main {Assembly}::{Class Full Name}::{Method}`

Exemple :
```
Apache.OpenWhisk.Example.Dotnet::Apache.OpenWhisk.Example.Dotnet.Hello::Main
```

### Package de code .NET Core
{: #prep_dotnet_pkg}

Avant de commencer :
Pour compiler, tester et archiver des projets .NET Core, vous devez :
- Installer le logiciel [SDK .NET Core](https://dotnet.microsoft.com/download) en local.
- Définir la variable d'environnement `DOTNET_HOME` à l'emplacement où se trouve l'exécutable `dotnet`.



Pour inclure votre code dans un package :

  1. Créez un projet C# appelé `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      dotnet new classlib -n Apache.OpenWhisk.Example.Dotnet -lang "C#"
      ```
      {: pre}

  2. Accédez au répertoire `Apache.OpenWhisk.Example.Dotnet`.

      ```bash
      cd Apache.OpenWhisk.Example.Dotnet
      ```
      {: pre}

  3. Installez le package NuGet [Newtonsoft.Json](https://www.newtonsoft.com/json) comme suit.

      ```bash
      dotnet add package Newtonsoft.Json -v 12.0.1
      ```
      {: pre}

  4. Sauvegardez le code suivant dans un fichier nommé `Hello.cs`.

      ```csharp
      using System;
    using Newtonsoft.Json.Linq;

      namespace Apache.OpenWhisk.Example.Dotnet
    {
          public class Hello
        {
              public JObject Main(JObject args)
            {
                  string name = "stranger";
                if (args.ContainsKey("name")) {
                      name = args["name"].ToString();
                }
                JObject message = new JObject();
                message.Add("greeting", new JValue($"Hello, {name}!"));
                return (message);
            }
          }
      }
      ```
      {: codeblock}

  5. Compilez `Hello.cs` ainsi que d'autres fichiers et sorties dans le répertoire `out`.

      ```bash
      dotnet publish -c Release -o out
      ```
      {: pre}

  6. Accédez au répertoire out.

      ```bash
      cd out
      ```
      {: pre}

  7. Compressez les fichiers publiés.

      ```bash
      zip -r -0 ../helloDotNet.zip *
      ```
      {: pre}


