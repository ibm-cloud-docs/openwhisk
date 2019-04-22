---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-08"

keywords: ballerina, serverless, actions

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

# Création d'actions Ballerina
{: #ballerina-actions}

Les sections ci-après vous expliquent comment créer et appeler une action Ballerina unique, et comment ajouter des paramètres à cette action. Une action est une fonction Ballerina de niveau supérieur qui accepte et renvoie un objet JSON.  

Les actions Ballerina sont exécutées dans Ballerina [0.990.2](https://ballerina.io/downloads). Vous avez besoin d'une version compatible du compilateur localement disponible pour générer l'exécutable. Sans le compilateur Ballerina, vous ne pouvez pas créer d'action.

## Création et appel d'une action Ballerina

**Avant de commencer :** Créez un fichier appelé `hello.bal` avec le code source suivant.

```ballerina
import ballerina/io;

public function main(json data) returns json {
  json? name = data.name;
  if (name == null) {
    return { greeting: "Hello stranger!" };
  } else {
    return { greeting: "Hello " + name.toString() + "!" };
  }
}
```
{: codeblock}

Par défaut, la méthode d'entrée pour l'action est `main`. Vous pouvez spécifier cette variable lorsque vous créez l'action avec l'interface de ligne de commande `wsk` à l'aide de `--main`. 

**Remarque :** Etant donné que le compilateur Ballerina s'attend à la présence d'une fonction appelée `main` pour générer l'exécutable, votre fichier source doit inclure un espace réservé nommé `main`.

Pour créer une action appelée `hello`, procédez comme suit. 

1. Générez le fichier .balx. 
  ```
  ballerina build hello.bal
  ```
{: pre}

2. Créez l'action à l'aide du fichier .balx. 
  ```
  ibmcloud fn action create bello hello.balx --kind ballerina:0.990
  ```
{: pre}

3. Cependant, l'interface de ligne de commande ne détermine pas le type d'action d'après l'extension du fichier source. Vous devez spécifier le type de manière explicite. Pour les fichiers source `.balx`, l'action s'exécute à l'aide de l'environnement d'exécution Ballerina 0.990.2. 
  ```
  ibmcloud fn action invoke --result bello --param name World
  ```
{: pre}

Exemple de sortie : 
```json
{
  "greeting": "Hello World!"
      }
```
{: screen}
