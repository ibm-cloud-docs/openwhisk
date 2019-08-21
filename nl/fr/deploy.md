---

copyright:
  years: 2017, 2019
lastupdated: "2019-07-12"

keywords: deploying actions, manifest, manifest file, functions

subcollection: cloud-functions

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:gif: data-image-type='gif'}


# Déploiement d'entités avec un fichier manifeste
{: #deploy}

Vous pouvez utiliser {{site.data.keyword.openwhisk_short}} pour décrire et déployer toutes vos entités d'espace de nom à l'aide d'un fichier manifeste écrit en YAML. Vous pouvez utiliser ce fichier pour déployer toutes vos fonctions [Packages](/docs/openwhisk?topic=cloud-functions-pkg_ov), [Actions](/docs/openwhisk?topic=cloud-functions-actions), [Déclencheurs](/docs/openwhisk?topic=cloud-functions-triggers) et [Règles](/docs/openwhisk?topic=cloud-functions-rules) avec une seule commande. 

Le fichier manifeste décrit l'ensemble d'entités que vous souhaitez déployer ou dont vous voulez annuler le déploiement en tant que groupe. Le contenu du fichier manifeste doit être conforme à la [spécification YAML de déploiement OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#package-specification){: external}. Une fois qu'il est défini, vous pouvez utiliser votre fichier manifeste pour déployer ou redéployer un groupe d'entités Functions dans le même espace de nom Functions ou dans un autre. Vous pouvez utiliser les commandes de plug-in Functions `ibmcloud fn deploy` et `ibmcloud fn undeploy` pour déployer les entités Functions définies dans votre fichier manifeste ou annuler leur déploiement.

## Création de l'exemple d'API Hello World
{: #deploy_helloworld_example}

Cet exemple prend un code Node.js simple, `helloworld.js`, crée une action Web `hello_world` dans un package `hello_world_package` et définit une API REST pour cette action.
{: shortdesc}

1. Créez un fichier `helloworld.js` avec le code suivant.

    ```javascript
    function main() {
       return {body: 'Hello world'};
}
    ```
    {: codeblock}

    Le fichier manifeste de déploiement définit les variables suivantes :
    * Le nom du package.
    * Le nom de l'action.
    * L'annotation d'action qui indique qu'il s'agit d'une action Web.
    * Le nom de fichier du code d'action.
    * L'API avec le chemin de base `/hello`.
    * Le chemin de noeud final `/world`.

2. Créez le fichier `hello_world_manifest.yml`.

    ```yaml
    packages:
  hello_world_package:
    version: 1.0
    license: Apache-2.0
    actions:
      hello_world:
        function: helloworld.js
        web-export: true
    apis:
      hello-world:
        hello:
          world:
            hello_world:
              method: GET
              response: http
    ```
    {: codeblock}

3. Utilisez la commande `deploy` pour déployer le package, l'action et l'API.

    ```sh
    ibmcloud fn deploy --manifest hello_world_manifest.yml
    ```
    {: pre}

4. Vous pouvez répertorier les actions, packages et API pour confirmer que les trois entités attendues ont été créées avec succès.

    1. Répertoriez les actions à l'aide de la commande suivante.

      ```sh
      ibmcloud fn action list
      ```
      {: pre}

    2. Répertoriez les packages à l'aide de la commande suivante.

      ```sh
      ibmcloud fn package list
      ```
      {: pre}

    3. Répertoriez les API à l'aide de la commande suivante.

      ```sh
      ibmcloud fn api list
      ```
      {: pre}

5. Appelez l'API.

    ```sh
    curl URL-FROM-API-LIST-OUTPUT
    ```
    {: codeblock}

Facultatif : vous pouvez annuler le déploiement des mêmes entités à l'aide de la commande `undeploy`.

```sh
ibmcloud fn undeploy --manifest hello_world_manifest.yml
```
{: codeblock}

## Autres exemples de déploiement OpenWhisk
{: more_deploy_examples}

Le déploiement de Functions se fonde sur le projet de déploiement OpenWhisk, qui contient
[plusieurs exemples de manifeste de déploiement](https://github.com/apache/incubator-openwhisk-wskdeploy/blob/master/docs/programming_guide.md#guided-examples){: external} qui peuvent être utilisés dans Functions.  Vous pouvez utiliser la commande `ibmcloud fn deploy` au lieu de `wskdeploy`.

## Spécification du manifeste de déploiement
{: manifest_specification}

Les manifestes de déploiement Functions doivent être conformes à la spécification du manifeste de déploiement OpenWhisk. Pour plus de détails, consultez la [documentation relative à la spécification du manifeste de déploiement OpenWhisk](https://github.com/apache/incubator-openwhisk-wskdeploy/tree/master/specification#openwhisk-packaging-specification){: external}.




