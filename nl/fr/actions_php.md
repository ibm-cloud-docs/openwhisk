---

copyright:
  years: 2017, 2019
lastupdated: "2019-03-22"

keywords: actions, serverless, php

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

# Création d'actions PHP
{: #creating-php-actions}

Les sections ci-après vous expliquent comment créer et appeler une action PHP unique, et comment ajouter des paramètres à cette action.
{: shortdesc}

## Création et appel d'une action PHP
{: #actions_php_invoke}
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
    * Par défaut, la méthode d'entrée pour l'action est `main`, mais elle peut être spécifiée lorsque vous créez l'action avec l'interface de ligne de commande `ibmcloud fn` en utilisant l'indicateur `--main`.

2. Créez une action nommée `helloPHP`.

    ```
    ibmcloud fn action create helloPHP hello.php
    ```
    {: pre}

    Le type d'action est déterminé à l'aide de l'extension du fichier source. Pour les fichiers source `.php`, l'action est lancée dans un environnement d'exécution PHP 7.3. Pour plus d'informations sur l'environnement d'exécution PHP, voir [Contextes d'exécution](https://cloud.ibm.com/docs/openwhisk?topic=cloud-functions-runtimes#openwhisk_ref_php).
  

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

## Conditionnement d'actions PHP dans des fichiers .zip
{: #actions_php_zip}
{: #openwhisk_actions_php_zip}

Vous pouvez conditionner une action PHP, ainsi que pour d'autres fichiers et packages dépendants, dans un fichier .zip. Par exemple, conditionnez une action avec un second fichier appelé `helper.php` :

1. Créez une archive contenant vos fichiers source. **Remarque **: Le fichier source qui contient le point d'entrée doit être nommé `index.php`.

    ```bash
    zip -r helloPHP.zip index.php helper.php
    ```
    {: pre}

2. Créez l'action.

    ```bash
    ibmcloud fn action create helloPHP --kind php:7.3 helloPHP.zip
    ```
    {: pre}

