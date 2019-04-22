---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-15"

keywords: docker, actions, serverless

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

# Création d'actions Go
{: #creating-go-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Go unique, et comment ajouter des paramètres à cette action. Vous pouvez exécuter des actions Go à l'aide de Go 1.11. Pour utiliser cet environnement d'exécution, spécifiez le paramètre d'interface de ligne de commande `ibmcloud fn` `--kind go:1.11` lors de la création ou de la mise à jour d'une action.
{: shortdesc}

## Création et appel d'une action Go
{: #invoking-go-actions}

Une action Go est simplement une fonction publique provenant du package `main`. Utilisez un fichier unique à des fins de test rapide ou de développement. Pour les applications de production, [précompilez vos actions Go dans un exécutable](#packaging-go-actions) pour améliorer les performances ou prendre en charge plusieurs fichiers source, notamment des bibliothèques fournisseur.
{: shortdesc}

Créez une action Go. 

1. Sauvegardez le code suivant dans un fichier nommé `hello.go`. Dans l'exemple, la fonction s'appelle `Main`, mais vous pouvez modifier son nom à l'aide de l'indicateur `--main`. Si vous renommez la fonction, le nom doit commencer par une majuscule et ne doit pas correspondre au terme `main`, qui est le nom du package Go. 

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

    </br>
    La signature attendue pour une fonction `Main` est :

    ```go
    func Main(event map[string]interface{}) map[string]interface{}
    ```
    {: codeblock}

2. Créez ou mettez à jour une action appelée `helloGo`. 

    ```bash
    ibmcloud fn action update helloGo hello.go --kind go:1.11
    ```
    {: pre}

3. Appelez l'action.

    ```bash
    ibmcloud fn action invoke --result helloGo --param name gopher
    ```
    {: pre}

    Exemple de sortie :
    ```json
      {
          "greeting": "Hello gopher!"
      }
    ```
    {: screen}

## Conditionnement d'une action sous forme d'exécutable Go
{: #packaging-go-actions}

Bien que vous puissiez créer un fichier binaire sur une plateforme Go en effectuant une compilation croisée avec `GOOS=Linux` et `GOARCH=amd64`, utilisez la fonction de précompilation qui est incorporée dans l'image du conteneur d'exécution. Vous pouvez conditionner [plusieurs fichiers source](#multiple-packages-go-actions) ou [bibliothèques fournisseur](#vendor-libs-go-actions).
{: shortdesc}

### Utilisation de plusieurs fichiers source du package
{: #multiple-packages-go-actions}

Pour utiliser plusieurs fichiers source du package, servez-vous d'un répertoire `src` de niveau supérieur, placez les fichiers source qui appartiennent au package principal à la racine de `src` ou à l'intérieur d'un répertoire `main` et créez des répertoires pour d'autres packages. Par exemple, le package `hello` devient le répertoire `src/hello`.
{: shortdesc}

```
go-action-hello/
└── src
    ├── hello
    │   └── hello.go
    └── main
        └── main.go
```
{: screen}

Avec cette présentation, vous pouvez importer des sous-packages (`import "hello"`) comme illustré dans l'exemple suivant. 

Vous pouvez compiler localement en définissant la variable `GOPATH` sur le parent du répertoire `src`. Si vous utilisez VSCode, vous devez définir le paramètre `go.inferGopath` sur `true`.
{: note}

Exemple de `main/main.go` :

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

Vous pouvez compiler en utilisant l'environnement d'exécution. Créez un fichier .zip pour archiver le contenu du répertoire `src`. **N'incluez pas** le répertoire de projet de niveau supérieur `go-action-project/`. Pour créer l'archive .zip `hello-src.zip` : 

```bash
cd src
zip -r ../hello-src.zip *
cd ..
```
{: pre}

Compilez et conditionnez l'exécutable Go sous la forme `exec` dans la racine de l'archive .zip. Générez l'archive `hello-bin.zip` en exécutant la commande ci-dessous. Cette procédure suppose que l'interface de ligne de commande Docker est installée sur votre poste de travail et `docker` dans votre chemin `PATH`. 

```bash
 docker run -i openwhisk/actionloop-golang-v1.11 -compile main <hello-src.zip >hello-bin.zip
```
{: pre}

Dans cet exemple, la fonction main est `-compile main`. Pour utiliser une autre fonction en tant que fonction main, modifiez la valeur de `-compile`.
La fonction main est sélectionnée au moment de la compilation. Lors de la précompilation, `ibmcloud fn action [update | create]` ignore le paramètre `--main`.

Le conteneur extrait le contenu du fichier source .zip dans `stdin`, le compile et crée une nouvelle archive .zip avec l'exécutable `exec` dans la racine. Le contenu de l'archive .zip est transféré vers `stdout`, qui est redirigé vers l'archive `hello-bin.zip` à déployer en tant qu'action Go. 

Vous pouvez maintenant mettre à jour votre action pour la production à l'aide de l'interface de ligne de commande et de la nouvelle archive .zip `hello-bin.zip`.

```bash
ibmcloud fn action update helloGo hello-bin.zip --kind go:1.11
```
{: codeblock}

### Utilisation des bibliothèques fournisseur
{: #vendor-libs-go-actions}

Vous pouvez inclure des dépendances en alimentant un répertoire `vendor` dans l'archive source `zip` lorsque vous compilez l'action Go. Le répertoire `vendor` ne fonctionne pas au niveau supérieur. Vous devez placer le répertoire `vendor` dans `src/`, à l'intérieur d'un répertoire de package.
{: shortdesc}

En reprenant l'exemple précédent, utilisez le package de journaux `logrus` dans `hello.go`. 

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

Vous pouvez automatiser ce processus à l'aide d'un outil de déploiement ou d'un script tel qu'un fichier [Makefile](#makefile).
{: tip}

### Utilisation d'un fichier Makefile pour les actions Go
{: #makefile}

En reprenant l'exemple précédent, votre répertoire de projet se présente comme suit.

```bash
go-action-hello/
├── Makefile
└── src
    ├── hello
    │   ├── Gopkg.toml
    │   ├── hello.go
    │   └── vendor/
    └── main
        └── main.go
```
{: screen}

1. Créez le fichier `Makefile` pour automatiser le processus de déploiement. 

```Makefile
GO_COMPILER?=openwhisk/actionloop-golang-v1.11
CLI?=ibmcloud fn
MAIN=main
APP=hello
SRCS=$(MAIN)/$(MAIN).go $(APP)/$(APP).go
VENDORS=$(APP)/vendor
NAME=go-action-$(APP)
BINZIP=$(APP)-bin.zip
SRCZIP=$(APP)-src.zip

deploy: $(BINZIP)
	$(CLI) action update $(NAME) $(BINZIP) --main $(MAIN) --kind go:1.11

$(BINZIP): $(SRCZIP)
	docker run -i $(GO_COMPILER) -compile $(MAIN) <$(SRCZIP) >$(BINZIP)

$(SRCZIP): src/$(VENDORS)
	cd src ; zip ../$(SRCZIP) -r $(SRCS) $(VENDORS)

src/%/vendor:
	cd $(@D) ; DEPPROJECTROOT=$(realpath $(@D)/../..) dep ensure

clean:
	-rm -rf $(BINZIP) $(SRCZIP) $(VENDORS)

invoke:
	$(CLI) action invoke $(NAME) -r -p name Gopher
```
{: codeblock}

2. Supprimez les archives .zip et le répertoire du fournisseur. 

```bash
make clean
```
{: pre}

3. Alimentez le répertoire du fournisseur, créez le fichier .zip source, compilez le code source, archivez l'exécutable dans un fichier .zip et déployez l'action Go en exécutant : 

```bash
make deploy
```
{: pre}

4. Une fois que `go-action-hello` est créé, appelez l'action. 

```bash
ibmcloud fn action invoke go-action-hello -r -p name Go
```
{: pre}

Exemple de sortie :

```json
    {
        "greeting": "Hello Go"
    }
```
{: screen}
